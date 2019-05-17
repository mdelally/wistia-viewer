# wistia-viewer

## Connecting to Wistia Account
First, you must create an API token in your Wistia account settings page (**Account>Settings>API Access**).

Once you have your password for the token, you must add it to your environment file.
```
echo "VUE_APP_WISTIA_TOKEN='{ YOUR_PASSWORD }'" > .env.local
```
Or for production...
```
echo "VUE_APP_WISTIA_TOKEN='{ YOUR_PASSWORD }'" > .env.production
```

This is all you need to link up and pull all your Wistia videos into the widget.

## Development setup
```
npm install
```

### Compiles and hot-reloads for development
```
npm run serve
```

### Compiles and minifies for production
```
npm run build
```

### Run your tests
```
npm run test
```

### Lints and fixes files
```
npm run lint
```
