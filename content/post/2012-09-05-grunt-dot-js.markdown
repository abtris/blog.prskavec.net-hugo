---
tags: 
 - javascript
comments: true
date: 2012-09-05T00:00:00Z
title: Grunt.js
url: /2012/09/05/grunt-dot-js/
---

Grunt.js je nástroj pro tvorbu ukolů. Obdobný nástroj existuje pro každý programovací jazyk a často nejenom jeden.

<!--more-->

Osobně mám nejraději asi klasický Makefile pro jeho jednoduchost. Stačí do něj napsat příkazy co pustíte v shellu takže to víceméně může být cokoliv. Makefile má jednoduchou strukturu, obsahuje label a potom vlastní příkaz. Při zadání `make` se spustí label označený `all`.

    all:
            make run
    deploy:
            git push heroku master
    run:
            coffee app.coffee
    css:
            compass compile
    logs:
            heroku addons:open loggly

Grunt.js je napsaný v node.js, instaluje se pomocí `npm install grunt`. Můžete si psát vlastní úkoly a pluginy, spoustu jich najdete hotových pomocí `npm search grunt-`.

V příkladu, který uvádím se používá pluginy compass, coffeelint a exec, nahrávají se pluginy pomocí `loadNpmTasks('name')`. Vlastní definované tasky můžete nahrát pomocí `loadTasks('dir')`. Nakonec po konfiguraci je potřeba zaregistrovat `registerTask('default', 'watch')`.

    module.exports = function(grunt) {
      // Load tasks
      grunt.loadNpmTasks('grunt-coffeelint');
      grunt.loadNpmTasks('grunt-compass');
      grunt.loadNpmTasks('grunt-exec');
      // Project configuration.
      grunt.initConfig({
        compass: {
          dev: {
            src: 'sass',
            dest: 'public/stylesheets',
            outputstyle: 'expanded',
            linecomments: true
          },
          prod: {
            src: 'sass',
            dest: 'public/stylesheets',
            outputstyle: 'compressed',
            linecomments: false,
            forcecompile: true
          }
        },
        coffee: {
          app: ['app.coffee','lib/*.coffee']
        },
        watch: { // for development run 'grunt watch'
          app: {
            files: ['app.coffee', 'lib/*.coffee'],
            tasks: ['coffee:app', 'coffeelint:dev']
          },
          compass: {
            files: ['sass/**/*.scss'],
            tasks: ['compass:dev']
          }
        },
        coffeelint: {
          one: {
            files: ['app.coffee','lib/*.coffee'],
            options: {
              indentation: {
                value: 2,
                level: "error"
              }
            }
          }
        },
        coffeelintOptions: {
          max_line_length: {
            value: 100,
            level: 'error'
          }
        },
        exec: {
          deploy: {
            command: 'git push heroku master',
            stdout: true,
            stderr: true
          },
          logs: {
            command: 'heroku addons:open loggly'
          },
          run: {
            command: 'coffee app.coffee',
            stdout: true,
            stderr: true
          }
        }
      });

      // Load local tasks.
      grunt.loadTasks('tasks');
      // Default task.
      grunt.registerTask('default', 'watch');
    };

Grunt.js umí generovat nějaké konfigurace pomocí `grunt init:template`. Základní templates jsou: `commonjs`, `gruntfile` , `gruntplugin`, `jquery`, `node`. Například vytvořit node.js modul včetně testů uděláte pomocí:

    grunt init:node

Vlastní grunt.js obsahuje některé základní tasky už v základním balíčku, které můžete velmi dobře použít.

- concat (spojování souborů)
- init (vytvoření předefinovaných konfigurací)
- lint (validace JSHint)
- min (UglifyJS)
- qunit
- server
- test
- watch

Grunt.js se dá dobře použít na vytvoření buildu, kde spojíte všechny javascriptové soubory. Provést minifikaci a kontrolu například pomocí JSHint. Jako ukázku uvádím [grunt.js z NGX library](https://github.com/lmc-eu/ngx-library).

    module.exports = function(grunt) {
        // tasks
        grunt.loadTasks('build/tasks');
        grunt.loadNpmTasks('grunt-recess');

        // configuration
        grunt.initConfig({
            pkg: '<json:package.json>',
            meta: {
                banner: '/**\n' +
                    ' * <%= pkg.description %>\n' +
                    ' * @version v<%= pkg.version %> - ' + '<%= grunt.template.today("yyyy-mm-dd") %>\n' +
                    ' * @link <%= pkg.homepage %>\n' +
                    ' * @license MIT License, http://www.opensource.org/licenses/MIT\n' +
                    ' */'
            },
            concat: {
                dist: {
                    src: ['<banner:meta.banner>', 'src/ngx.js', 'src/**/*.js'],
                    dest: 'dist/ngx.js'
                }
            },
            clean: {
                dist: ['dist/']
            },
            copy: {
                dist: {
                    files: {
                        'dist/templates/ui': ['src/modules/ui/**/*.html'],
                        'dist/libs': ['libs/**']
                    }
                }
            },
            min: {
                dist: {
                    src: ['<banner:meta.banner>', '<config:concat.dist.dest>'],
                    dest: 'dist/ngx.min.js'
                }
            },
            recess: {
                dist_css: {
                    src: 'src/**/*.less',
                    dest: 'dist/styles/ngx.css',
                    options: {
                        compile: true
                    }
                },
                dist_min: {
                    src: '<config:recess.dist_css.dest>',
                    dest: 'dist/styles/ngx.min.css',
                    options: {
                        compress: true
                    }
                }
            },
            lint: {
                files: ['grunt.js', 'src/ngx.js', 'src/lang/*.js', 'src/modules/**/*.js']
            },
            watch: {
                scripts: {
                    files: ['grunt.js', 'src/*.js', 'src/**/*.js'],
                    tasks: 'lint concat min'
                },
                styles: {
                    files: ['src/**/*.less'],
                    tasks: 'recess'
                },
                templates: {
                    files: ['src/modules/**/*.html'],
                    tasks: 'copy'
                }
            }
        });

        // default task
        grunt.registerTask('default', 'lint clean concat copy min recess');

    };

Grunt.js využívá například [Yeoman](http://yeoman.io), na tyto úkoly.
- kontrolu javascriptu pomocí JSHint
- kompilace CoffeeScriptu a SASS souborů pro produkci
- používá [r.js](https://github.com/jrburke/r.js/) ke kompilaci a optimalizaci
- spojení a minifikaci skriptů a stylů
- komprese obrázků pomocí OptiPNG pro PNG a JPEGtran-turbo pro JPEG.
- spuštění unit testů proti headless WebKit browser (PhantomJS)
- vytvoření Application Cache manifest via [Confess.js](https://github.com/jamesgpearce/confess)

Použití je všestrané a pokryje to celé spektrum práce s aplikacemi v javascriptu.


