"serve": {
  "executor": "@nx/webpack:dev-server",
  "defaultConfiguration": "development",
  "options": {
    "buildTarget": "ipa-config-ui:build",
    "hmr": true,
    "devServer": {
      "proxy": {
        "^/payments": {
          "target": "http://localhost:8080",
          "changeOrigin": true,
          "secure": false,
          "logLevel": "debug"
        }
      }
    }
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
}
