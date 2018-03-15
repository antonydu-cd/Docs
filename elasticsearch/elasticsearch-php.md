# composer 安装
    {
        "require": {
            "elasticsearch/elasticsearch": "~5.0"
        }
    }
# 创建客户端
    require 'vendor/autoload.php';

    use Elasticsearch\ClientBuilder;

    $client = ClientBuilder::create()->build();

# 指定HOST
    $hosts = [
        '192.168.1.1:9200',         // IP + Port
        '192.168.1.2',              // Just IP
        'mydomain.server.com:9201', // Domain + Port
        'mydomain2.server.com',     // Just Domain
        'https://localhost',        // SSL to localhost
        'https://192.168.1.3:9200'  // SSL to IP + Port
    ];

    $client = ClientBuilder::create()       // Instantiate a new ClientBuilder
                    ->setHosts($hosts)      // Set the hosts
                    ->build();              // Build the client object
# Set retries
    $client = ClientBuilder::create()
                    ->setRetries(2)
                    ->build();
                
# Enabling the logger
    {
        "require": {
            ...
            "elasticsearch/elasticsearch" : "~5.0",
            "monolog/monolog": "~1.0"
        }
    }

    use Monolog\Logger;
    $logger = ClientBuilder::defaultLogger('path/to/your.log', Logger::INFO);

    $client = ClientBuilder::create()       // Instantiate a new ClientBuilder
                ->setLogger($logger)        // Set the logger with a default logger
                ->build();                  // Build the client object

# Configure the HTTP Handler
    $defaultHandler = ClientBuilder::defaultHandler();
    $singleHandler  = ClientBuilder::singleHandler();
    $multiHandler   = ClientBuilder::multiHandler();
    $customHandler  = new MyCustomHandler();

    $client = ClientBuilder::create()
                ->setHandler($defaultHandler)
                ->build();

# Setting the Connection Pool
    $connectionPool = '\Elasticsearch\ConnectionPool\StaticNoPingConnectionPool';
    $client = ClientBuilder::create()
                ->setConnectionPool($connectionPool)
                ->build(); 