{
  "name": "ipa-config-ui",
  "$schema": "node_modules/nx/schemas/project-schema.json",
  "includedScripts": [],
  "sourceRoot": "./src",
  "projectType": "application",
  "tags": [],
  "// targets": "to see all targets run: nx show project ipa-config-ui --web",
  "targets": {

    "build": {
      "executor": "@nx/webpack:webpack",
      "outputs": ["{options.outputPath}"],
      "defaultConfiguration": "dev",
      "options": {
        "compiler": "babel",
        "outputPath": "dist/ipa-config-ui",
        "index": "src/index.html",
        "baseHref": "/",
        "main": "src/main.tsx",
        "tsConfig": "tsconfig.app.json",
        "assets": ["src/favicon.ico", "src/assets"],
        "scripts": [],
        "isolatedConfig": true,
        "webpackConfig": "webpack.config.js"
      },
      "configurations": {
        "dev": {
          "fileReplacements": [
            {
              "replace": "src/environments/environment.ts",
              "with": "src/environments/environment.dev.ts"
            }
          ],
          "optimization": true,
          "outputHashing": "all",
          "sourceMap": false,
          "namedChunks": false,
          "extractLicenses": true,
          "vendorChunk": false
        },
        "qa": {
          "fileReplacements": [
            {
              "replace": "src/environments/environment.ts",
              "with": "src/environments/environment.qa.ts"
            }
          ],
          "optimization": true,
          "outputHashing": "all",
          "sourceMap": false,
          "namedChunks": false,
          "extractLicenses": true,
          "vendorChunk": false
        },
        "prod": {
          "fileReplacements": [
            {
              "replace": "src/environments/environment.ts",
              "with": "src/environments/environment.prod.ts"
            }
          ],
          "optimization": true,
          "outputHashing": "all",
          "sourceMap": false,
          "namedChunks": false,
          "extractLicenses": true,
          "vendorChunk": false
        }
      }
    },
    "serve": {
      "executor": "@nx/webpack:dev-server",
      "defaultConfiguration": "development",
      "options": {
        "buildTarget": "ipa-config-ui:build",
        "hmr": true
      },
      "configurations": {
        "development": {
          "buildTarget": "ipa-config-ui:build:development"
        },
        "production": {
          "buildTarget": "ipa-config-ui:build:production",
          "hmr": false
        }
      }
    },
    "lint": {
      "executor": "@nx/linter:eslint",
      "outputs": ["{options.outputFile}"],
      "options": {
        "lintFilePatterns": [
          "./src/**/*.{ts,tsx,js,jsx}",
          "./packages/**/*.{ts,tsx,js,jsx}"
        ]
      }
    },
    "serve-static": {
      "executor": "@nx/web:file-server",
      "options": {
        "buildTarget": "ipa-config-ui:build"
      }
    },
    "test": {
      "executor": "@nx/jest:jest",
      "outputs": ["{workspaceRoot}/coverage/{projectName}"],
      "options": {
        "jestConfig": "jest.config.ts",
        "passWithNoTests": true
      },
      "configurations": {
        "ci": {
          "ci": true,
          "codeCoverage": true
        }
      }
    }


  }
}
