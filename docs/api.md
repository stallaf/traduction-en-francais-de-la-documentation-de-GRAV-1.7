<h1 class="rem">API Grav</h1>

<h2 id="Table des matières">Table des matières.
<a href="#Table des matières" class="toc-anchor after"></a></h2>

NDT :Je renvoie ici le lecteur vers la [documentation officielle](https://learn.getgrav.org/17/api)

* \Grav\Common\Iterator
* \Grav\Common\Plugin
* \Grav\Common\Taxonomy
* \Grav\Common\Utils (abstract)
* \Grav\Common\Uri
* \Grav\Common\Assets
* \Grav\Common\Getters (abstract)
* \Grav\Common\Cache
* \Grav\Common\Theme
* \Grav\Common\Themes
* \Grav\Common\Yaml (abstract)
* \Grav\Common\Composer
* \Grav\Common\Browser
* \Grav\Common\Inflector
* \Grav\Common\Security
* \Grav\Common\Session
* \Grav\Common\Assets\Css
* \Grav\Common\Assets\Js
* \Grav\Common\Assets\InlineCss
* \Grav\Common\Assets\Pipeline
* \Grav\Common\Assets\BaseAsset (abstract)
* \Grav\Common\Assets\InlineJs
* \Grav\Common\Backup\Backups
* \Grav\Common\Config\CompiledConfig
* \Grav\Common\Config\CompiledLanguages
* \Grav\Common\Config\Setup
* \Grav\Common\Config\Config
* \Grav\Common\Config\ConfigFileFinder
* \Grav\Common\Config\CompiledBlueprints
* \Grav\Common\Config\Languages
* \Grav\Common\Data\Blueprints
* \Grav\Common\Data\Blueprint
* \Grav\Common\Data\DataInterface (interface)
* \Grav\Common\Data\ValidationException
* \Grav\Common\Data\Validation
* \Grav\Common\Data\Data
* \Grav\Common\Data\BlueprintSchema
* \Grav\Common\Errors\BareHandler
* \Grav\Common\Errors\SystemFacade
* \Grav\Common\Errors\Errors
* \Grav\Common\Errors\SimplePageHandler
* \Grav\Common\File\CompiledYamlFile
* \Grav\Common\File\CompiledJsonFile
* \Grav\Common\File\CompiledMarkdownFile
* \Grav\Common\Filesystem\RecursiveFolderFilterIterator
* \Grav\Common\Filesystem\Archiver (abstract)
* \Grav\Common\Filesystem\RecursiveDirectoryFilterIterator
* \Grav\Common\Filesystem\Folder (abstract)
* \Grav\Common\Filesystem\ZipArchiver
* \Grav\Common\Flex\Types\Generic\GenericCollection
* \Grav\Common\Flex\Types\Generic\GenericIndex
* \Grav\Common\Flex\Types\Generic\GenericObject
* \Grav\Common\Flex\Types\Pages\PageObject
* \Grav\Common\Flex\Types\Pages\PageCollection
* \Grav\Common\Flex\Types\Pages\PageIndex
* \Grav\Common\Flex\Types\Pages\Storage\PageStorage
* \Grav\Common\Flex\Types\UserGroups\UserGroupCollection
* \Grav\Common\Flex\Types\UserGroups\UserGroupIndex
* \Grav\Common\Flex\Types\UserGroups\UserGroupObject
* \Grav\Common\Flex\Types\Users\UserCollection
* \Grav\Common\Flex\Types\Users\UserIndex
* \Grav\Common\Flex\Types\Users\UserObject
* \Grav\Common\Flex\Types\Users\Storage\UserFolderStorage
* \Grav\Common\Flex\Types\Users\Storage\UserFileStorage
* \Grav\Common\Form\FormFlash
* \Grav\Common\GPM\Upgrader
* \Grav\Common\GPM\AbstractCollection (abstract)
* \Grav\Common\GPM\GPM
* \Grav\Common\GPM\Licenses
* \Grav\Common\GPM\Response
* \Grav\Common\GPM\Installer
* \Grav\Common\GPM\Common\CachedCollection
* \Grav\Common\GPM\Common\Package
* \Grav\Common\GPM\Common\AbstractPackageCollection (abstract)
* \Grav\Common\GPM\Local\Packages
* \Grav\Common\GPM\Local\Themes
* \Grav\Common\GPM\Local\Plugins
* \Grav\Common\GPM\Local\Package
* \Grav\Common\GPM\Local\AbstractPackageCollection (abstract)
* \Grav\Common\GPM\Remote\Packages
* \Grav\Common\GPM\Remote\Themes
* \Grav\Common\GPM\Remote\Plugins
* \Grav\Common\GPM\Remote\Package
* \Grav\Common\GPM\Remote\AbstractPackageCollection
* \Grav\Common\GPM\Remote\GravCore
* \Grav\Common\Helpers\Base32
* \Grav\Common\Helpers\Excerpts
* \Grav\Common\Helpers\Exif
* \Grav\Common\Helpers\Truncator
* \Grav\Common\Helpers\LogViewer
* \Grav\Common\Helpers\YamlLinter
* \Grav\Common\Language\Language
* \Grav\Common\Language\LanguageCodes
* \Grav\Common\Markdown\ParsedownExtra
* \Grav\Common\Markdown\Parsedown
* \Grav\Common\Media\Interfaces\VideoMediaInterface (interface)
* \Grav\Common\Media\Interfaces\MediaLinkInterface (interface)
* \Grav\Common\Media\Interfaces\MediaFileInterface (interface)
* \Grav\Common\Media\Interfaces\ImageManipulateInterface (interface)
* \Grav\Common\Media\Interfaces\MediaPlayerInterface (interface)
* \Grav\Common\Media\Interfaces\MediaCollectionInterface (interface)
* \Grav\Common\Media\Interfaces\ImageMediaInterface (interface)
* \Grav\Common\Media\Interfaces\MediaUploadInterface (interface)
* \Grav\Common\Media\Interfaces\MediaObjectInterface (interface)
* \Grav\Common\Media\Interfaces\MediaInterface (interface)
* \Grav\Common\Media\Interfaces\AudioMediaInterface (interface)
* \Grav\Common\Page\Collection
* \Grav\Common\Page\Pages
* \Grav\Common\Page\Media
* \Grav\Common\Page\Header
* \Grav\Common\Page\Types
* \Grav\Common\Page\Page
* \Grav\Common\Page\Interfaces\PageInterface (interface)
* \Grav\Common\Page\Interfaces\PageCollectionInterface (interface)
* \Grav\Common\Page\Interfaces\PageRoutableInterface (interface)
* \Grav\Common\Page\Interfaces\PageLegacyInterface (interface)
* \Grav\Common\Page\Interfaces\PageFormInterface (interface)
* \Grav\Common\Page\Interfaces\PageContentInterface (interface)
* \Grav\Common\Page\Interfaces\PagesSourceInterface (interface)
* \Grav\Common\Page\Interfaces\PageTranslateInterface (interface)
* \Grav\Common\Page\Markdown\Excerpts
* \Grav\Common\Page\Medium\AudioMedium
* \Grav\Common\Page\Medium\MediumFactory
* \Grav\Common\Page\Medium\VideoMedium
* \Grav\Common\Page\Medium\ThumbnailImageMedium
* \Grav\Common\Page\Medium\StaticImageMedium
* \Grav\Common\Page\Medium\AbstractMedia (abstract)
* \Grav\Common\Page\Medium\RenderableInterface (interface)
* \Grav\Common\Page\Medium\ImageMedium
* \Grav\Common\Page\Medium\Medium
* \Grav\Common\Page\Medium\ImageFile
* \Grav\Common\Page\Medium\GlobalMedia
* \Grav\Common\Processors\DebuggerAssetsProcessor
* \Grav\Common\Processors\PluginsProcessor
* \Grav\Common\Processors\ThemesProcessor
* \Grav\Common\Processors\ProcessorBase (abstract)
* \Grav\Common\Processors\AssetsProcessor
* \Grav\Common\Processors\PagesProcessor
* \Grav\Common\Processors\BackupsProcessor
* \Grav\Common\Processors\RequestProcessor
* \Grav\Common\Processors\RenderProcessor
* \Grav\Common\Processors\TwigProcessor
* \Grav\Common\Processors\TasksProcessor
* \Grav\Common\Processors\InitializeProcessor
* \Grav\Common\Processors\ProcessorInterface (interface)
* \Grav\Common\Processors\Events\RequestHandlerEvent
* \Grav\Common\Scheduler\Cron
* \Grav\Common\Scheduler\Job
* \Grav\Common\Scheduler\Scheduler
* \Grav\Common\Service\RequestServiceProvider
* \Grav\Common\Service\StreamsServiceProvider
* \Grav\Common\Service\BackupsServiceProvider
* \Grav\Common\Service\PagesServiceProvider
* \Grav\Common\Service\ErrorServiceProvider
* \Grav\Common\Service\FlexServiceProvider
* \Grav\Common\Service\AccountsServiceProvider
* \Grav\Common\Service\ConfigServiceProvider
* \Grav\Common\Service\SchedulerServiceProvider
* \Grav\Common\Service\FilesystemServiceProvider
* \Grav\Common\Service\LoggerServiceProvider
* \Grav\Common\Service\AssetsServiceProvider
* \Grav\Common\Service\OutputServiceProvider
* \Grav\Common\Service\SessionServiceProvider
* \Grav\Common\Service\TaskServiceProvider
* \Grav\Common\Service\InflectorServiceProvider
* \Grav\Common\Twig\TwigProfileProcessor
* \Grav\Common\Twig\TwigEnvironment
* \Grav\Common\Twig\TwigClockworkDataSource
* \Grav\Common\Twig\Twig
* \Grav\Common\Twig\Node\TwigNodeThrow
* \Grav\Common\Twig\Node\TwigNodeSwitch
* \Grav\Common\Twig\Node\TwigNodeStyle
* \Grav\Common\Twig\Node\TwigNodeMarkdown
* \Grav\Common\Twig\Node\TwigNodeScript
* \Grav\Common\Twig\Node\TwigNodeRender
* \Grav\Common\Twig\Node\TwigNodeCache
* \Grav\Common\Twig\Node\TwigNodeTryCatch
* \Grav\Common\Twig\TokenParser\TwigTokenParserRender
* \Grav\Common\Twig\TokenParser\TwigTokenParserCache
* \Grav\Common\Twig\TokenParser\TwigTokenParserTryCatch
* \Grav\Common\Twig\TokenParser\TwigTokenParserMarkdown
* \Grav\Common\Twig\TokenParser\TwigTokenParserSwitch
* \Grav\Common\Twig\TokenParser\TwigTokenParserStyle
* \Grav\Common\Twig\TokenParser\TwigTokenParserThrow
* \Grav\Common\Twig\TokenParser\TwigTokenParserScript
* \Grav\Common\User\Group
* \Grav\Common\User\Access
* \Grav\Common\User\Authentication (abstract)
* \Grav\Common\User\DataUser\UserCollection
* \Grav\Common\User\DataUser\User
* \Grav\Common\User\Interfaces\UserGroupInterface (interface)
* \Grav\Common\User\Interfaces\AuthorizeInterface (interface)
* \Grav\Common\User\Interfaces\UserInterface (interface)
* \Grav\Common\User\Interfaces\UserCollectionInterface (interface)
* \Grav\Console\ConsoleCommand
* \Grav\Console\Cli\PageSystemValidatorCommand
* \Grav\Console\Cli\LogViewerCommand
* \Grav\Console\Cli\CleanCommand
* \Grav\Console\Cli\ComposerCommand
* \Grav\Console\Cli\SchedulerCommand
* \Grav\Console\Cli\NewProjectCommand
* \Grav\Console\Cli\SandboxCommand
* \Grav\Console\Cli\InstallCommand
* \Grav\Console\Cli\YamlLinterCommand
* \Grav\Console\Cli\ClearCacheCommand
* \Grav\Console\Cli\BackupCommand
* \Grav\Console\Cli\SecurityCommand
* \Grav\Console\Cli\ServerCommand
* \Grav\Console\Gpm\DirectInstallCommand
* \Grav\Console\Gpm\IndexCommand
* \Grav\Console\Gpm\UninstallCommand
* \Grav\Console\Gpm\InstallCommand
* \Grav\Console\Gpm\VersionCommand
* \Grav\Console\Gpm\SelfupgradeCommand
* \Grav\Console\Gpm\UpdateCommand
* \Grav\Console\Gpm\InfoCommand
* \Grav\Console\TerminalObjects\Table
* \Grav\Events\FlexRegisterEvent
* \Grav\Events\PluginsLoadedEvent
* \Grav\Events\PermissionsRegisterEvent
* \Grav\Events\SessionStartEvent
* \Grav\Framework\Acl\Action
* \Grav\Framework\Acl\PermissionsReader
* \Grav\Framework\Acl\RecursiveActionIterator
* \Grav\Framework\Acl\Access
* \Grav\Framework\Acl\Permissions
* \Grav\Framework\Cache\CacheInterface (interface)
* \Grav\Framework\Cache\AbstractCache (abstract)
* \Grav\Framework\Cache\Adapter\FileCache
* \Grav\Framework\Cache\Adapter\SessionCache
* \Grav\Framework\Cache\Adapter\ChainCache
* \Grav\Framework\Cache\Adapter\MemoryCache
* \Grav\Framework\Cache\Adapter\DoctrineCache
* \Grav\Framework\Cache\Exception\InvalidArgumentException
* \Grav\Framework\Cache\Exception\CacheException
* \Grav\Framework\Collection\AbstractIndexCollection (abstract)
* \Grav\Framework\Collection\CollectionInterface (interface)
* \Grav\Framework\Collection\AbstractLazyCollection (abstract)
* \Grav\Framework\Collection\FileCollectionInterface (interface)
* \Grav\Framework\Collection\ArrayCollection
* \Grav\Framework\Collection\AbstractFileCollection
* \Grav\Framework\Collection\FileCollection
* \Grav\Framework\ContentBlock\HtmlBlock
* \Grav\Framework\ContentBlock\ContentBlockInterface (interface)
* \Grav\Framework\ContentBlock\ContentBlock
* \Grav\Framework\ContentBlock\HtmlBlockInterface (interface)
* \Grav\Framework\DI\Container
* \Grav\Framework\File\DataFile
* \Grav\Framework\File\AbstractFile
* \Grav\Framework\File\File
* \Grav\Framework\File\MarkdownFile
* \Grav\Framework\File\YamlFile
* \Grav\Framework\File\CsvFile
* \Grav\Framework\File\IniFile
* \Grav\Framework\File\JsonFile
* \Grav\Framework\File\Formatter\YamlFormatter
* \Grav\Framework\File\Formatter\AbstractFormatter (abstract)
* \Grav\Framework\File\Formatter\IniFormatter
* \Grav\Framework\File\Formatter\FormatterInterface (interface)
* \Grav\Framework\File\Formatter\JsonFormatter
* \Grav\Framework\File\Formatter\SerializeFormatter
* \Grav\Framework\File\Formatter\CsvFormatter
* \Grav\Framework\File\Formatter\MarkdownFormatter
* \Grav\Framework\File\Interfaces\FileFormatterInterface (interface)
* \Grav\Framework\File\Interfaces\FileInterface (interface)
* \Grav\Framework\Filesystem\Filesystem
* \Grav\Framework\Filesystem\Interfaces\FilesystemInterface (interface)
* \Grav\Framework\Flex\Flex
* \Grav\Framework\Flex\FlexForm
* \Grav\Framework\Flex\FlexFormFlash
* \Grav\Framework\Flex\FlexDirectoryForm
* \Grav\Framework\Flex\FlexDirectory
* \Grav\Framework\Flex\Interfaces\FlexTranslateInterface (interface)
* \Grav\Framework\Flex\Interfaces\FlexFormInterface (interface)
* \Grav\Framework\Flex\Interfaces\FlexInterface (interface)
* \Grav\Framework\Flex\Interfaces\FlexObjectInterface (interface)
* \Grav\Framework\Flex\Interfaces\FlexObjectFormInterface (interface)
* \Grav\Framework\Flex\Interfaces\FlexCollectionInterface (interface)
* \Grav\Framework\Flex\Interfaces\FlexAuthorizeInterface (interface)
* \Grav\Framework\Flex\Interfaces\FlexIndexInterface (interface)
* \Grav\Framework\Flex\Interfaces\FlexStorageInterface (interface)
* \Grav\Framework\Flex\Interfaces\FlexCommonInterface (interface)
* \Grav\Framework\Flex\Interfaces\FlexDirectoryFormInterface (interface)
* \Grav\Framework\Flex\Pages\FlexPageCollection
* \Grav\Framework\Flex\Pages\FlexPageObject
* \Grav\Framework\Flex\Pages\FlexPageIndex
* \Grav\Framework\Flex\Storage\SimpleStorage
* \Grav\Framework\Flex\Storage\FolderStorage
* \Grav\Framework\Flex\Storage\AbstractFilesystemStorage (abstract)
* \Grav\Framework\Flex\Storage\FileStorage
* \Grav\Framework\Form\FormFlash
* \Grav\Framework\Form\FormFlashFile
* \Grav\Framework\Form\Interfaces\FormFlashInterface (interface)
* \Grav\Framework\Form\Interfaces\FormFactoryInterface (interface)
* \Grav\Framework\Form\Interfaces\FormInterface (interface)
* \Grav\Framework\Interfaces\RenderInterface (interface)
* \Grav\Framework\Media\Interfaces\MediaManipulationInterface (interface)
* \Grav\Framework\Media\Interfaces\MediaCollectionInterface (interface)
* \Grav\Framework\Media\Interfaces\MediaObjectInterface (interface)
* \Grav\Framework\Media\Interfaces\MediaInterface (interface)
* \Grav\Framework\Object\ArrayObject
* \Grav\Framework\Object\ObjectIndex (abstract)
* \Grav\Framework\Object\LazyObject
* \Grav\Framework\Object\ObjectCollection
* \Grav\Framework\Object\PropertyObject
* \Grav\Framework\Object\Collection\ObjectExpressionVisitor
* \Grav\Framework\Object\Interfaces\ObjectInterface (interface)
* \Grav\Framework\Object\Interfaces\NestedObjectCollectionInterface (interface)
* \Grav\Framework\Object\Interfaces\NestedObjectInterface (interface)
* \Grav\Framework\Object\Interfaces\ObjectCollectionInterface (interface)
* \Grav\Framework\Pagination\AbstractPaginationPage (abstract)
* \Grav\Framework\Pagination\Pagination
* \Grav\Framework\Pagination\AbstractPagination
* \Grav\Framework\Pagination\PaginationPage
* \Grav\Framework\Pagination\Interfaces\PaginationInterface (interface)
* \Gav\Framework\Pagination\Interfaces\PaginationPageInterface (interface)
* \Grav\Framework\Psr7\Request
* \Grav\Framework\Psr7\ServerRequest
* \Grav\Framework\Psr7\UploadedFile
* \Grav\Framework\Psr7\Uri
* \Grav\Framework\Psr7\Stream
* \Grav\Framework\Psr7\Response
* \Grav\Framework\Psr7\AbstractUri (abstract)
* \Grav\Framework\RequestHandler\RequestHandler
* \Grav\Framework\RequestHandler\Exception\InvalidArgumentException
* \Grav\Framework\RequestHandler\Exception\NotFoundException
* \Grav\Framework\RequestHandler\Exception\RequestException
* \Grav\Framework\RequestHandler\Exception\PageExpiredException
* \Grav\Framework\RequestHandler\Exception\NotHandledException
* \Grav\Framework\RequestHandler\Middlewares\Exceptions
* \Grav\Framework\Route\RouteFactory
* \Grav\Framework\Route\Route
* \Grav\Framework\Session\SessionInterface (interface)
* \Grav\Framework\Session\Session
* \Grav\Framework\Session\Exceptions\SessionException
* \Grav\Framework\Uri\UriFactory
* \Grav\Framework\Uri\Uri
* \Grav\Framework\Uri\UriPartsFilter
* \Grav\Installer\Install

