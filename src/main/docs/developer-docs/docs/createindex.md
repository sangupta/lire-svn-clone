# Creating an Index with Lire
Use the `GenericDocumentBuilder` to create a `DocumentBuilder`, for instance with `new GenericDocumentBuilder(CEDD.class)`. Add images to an index using following steps:

  * With this `DocumentBuilder` Lucene documents can be created from images, for instance with `builder.createDocument(FileInputStream, String)`.
  * Eventually enrich the documents with your own data.
  * Add the document to an index.

There are quite a lot of different features available in LIRE. The `GenericDocumentBuilder` can take any `LireFeature`
implementation and create a builder class from it. If you need more than one feature, then take a look at the
`ChainedDocumentBuilder` (see below).

## Sample Code - Simple Indexing

    /**
     * Simple index creation with Lire
     *
     * @author Mathias Lux, mathias@juggle.at
     */
    public class Indexer {
        public static void main(String[] args) throws IOException {
            // Checking if arg[0] is there and if it is a directory.
            boolean passed = false;
            if (args.length > 0) {
                File f = new File(args[0]);
                System.out.println("Indexing images in " + args[0]);
                if (f.exists() && f.isDirectory()) passed = true;
            }
            if (!passed) {
                System.out.println("No directory given as first argument.");
                System.out.println("Run \"Indexer <directory>\" to index files of a directory.");
                System.exit(1);
            }
            // Getting all images from a directory and its sub directories.
            ArrayList<String> images = FileUtils.getAllImages(new File(args[0]), true);

            // Creating a CEDD document builder and indexing all files.
            DocumentBuilder builder = new GenericDocumentBuilder(CEDD.class);
            // Creating an Lucene IndexWriter
            IndexWriterConfig conf = new IndexWriterConfig(LuceneUtils.LUCENE_VERSION,
                    new WhitespaceAnalyzer(LuceneUtils.LUCENE_VERSION));
            IndexWriter iw = new IndexWriter(FSDirectory.open(new File("index")), conf);
            // Iterating through images building the low level features
            for (Iterator<String> it = images.iterator(); it.hasNext(); ) {
                String imageFilePath = it.next();
                System.out.println("Indexing " + imageFilePath);
                try {
                    BufferedImage img = ImageIO.read(new FileInputStream(imageFilePath));
                    Document document = builder.createDocument(img, imageFilePath);
                    iw.addDocument(document);
                } catch (Exception e) {
                    System.err.println("Error reading image or indexing it.");
                    e.printStackTrace();
                }
            }
            // closing the IndexWriter
            iw.close();
            System.out.println("Finished indexing.");
        }
    }


## Sample Code - ChainedDocumentBuilder
Use a new `ChainedDocumentBuilder` and add `DocumentBuilder` classes you like, such as ...


    /**
     * Simple index creation with Lire
     *
     * @author Mathias Lux, mathias@juggle.at
     */
    public class Indexer {
        public static void main(String[] args) throws IOException {
            // Checking if arg[0] is there and if it is a directory.
            boolean passed = false;
            if (args.length > 0) {
                File f = new File(args[0]);
                System.out.println("Indexing images in " + args[0]);
                if (f.exists() && f.isDirectory()) passed = true;
            }
            if (!passed) {
                System.out.println("No directory given as first argument.");
                System.out.println("Run \"Indexer <directory>\" to index files of a directory.");
                System.exit(1);
            }
            // Getting all images from a directory and its sub directories.
            ArrayList<String> images = FileUtils.getAllImages(new File(args[0]), true);

            // Use multiple DocumentBuilder instances:
            ChainedDocumentBuilder builder = new ChainedDocumentBuilder();
            builder.addBuilder(new GenericDocumentBuilder(CEDD.class));
            builder.addBuilder(new GenericDocumentBuilder(FCTH.class));
            builder.addBuilder(new GenericDocumentBuilder(AutoColorCorrelogram.class));

            // Creating an Lucene IndexWriter
            IndexWriterConfig conf = new IndexWriterConfig(LuceneUtils.LUCENE_VERSION,
                    new WhitespaceAnalyzer(LuceneUtils.LUCENE_VERSION));
            IndexWriter iw = new IndexWriter(FSDirectory.open(new File("index")), conf);
            // Iterating through images building the low level features
            for (Iterator<String> it = images.iterator(); it.hasNext(); ) {
                String imageFilePath = it.next();
                System.out.println("Indexing " + imageFilePath);
                try {
                    BufferedImage img = ImageIO.read(new FileInputStream(imageFilePath));
                    Document document = builder.createDocument(img, imageFilePath);
                    iw.addDocument(document);
                } catch (Exception e) {
                    System.err.println("Error reading image or indexing it.");
                    e.printStackTrace();
                }
            }
            // closing the IndexWriter
            iw.close();
            System.out.println("Finished indexing.");
        }
    }

## Sample Code - Parallel Indexing
If you have multiple CPU cores you can use the parallel indexing tool. Note that with the option for the threads, you
just configure the number of consumer threads. There will be a monitor thread, a main thread and a producer thread too.
However, only the n consumer threads plus the one producer thread will create CPU load, whereas the producer will just
read and put it into a queue.


    /**
     * Parallel index creation with Lire
     *
     * @author Mathias Lux, mathias@juggle.at
     */
    // use ParallelIndexer to index all photos from args[0] into "index"
    // and use 6 threads (actually 7 with the I/O thread).
    ParallelIndexer indexer = new ParallelIndexer(6, "index", "c:/temp/images/") {
        // use this to add you preferred builders. For now we go for CEDD, FCTH and AutoColorCorrelogram
        public void addBuilders(ChainedDocumentBuilder builder) {
            builder.addBuilder(new GenericDocumentBuilder(CEDD.class));
            builder.addBuilder(new GenericDocumentBuilder(FCTH.class));
            builder.addBuilder(new GenericDocumentBuilder(AutoColorCorrelogram.class));
        }
    };
    indexer.run();
