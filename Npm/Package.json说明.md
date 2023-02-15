```json
{
  "name": "react-quick-component-library",//项目名称
  "version": "0.1.1",//版本号
  "private": false,//私有共有
  "description": "React components library",//描述
  "author": "MrRabbit1993",//作者
  "license": "MIT",//协议
  "keywords": ["Component","UI","React"],//关键字
  "homepage": "https://github.com/MrRabbit1993/react-ts.git",// 主页地址
  "repository": {
    "type": "git",//git模式
    "url": "https://github.com/MrRabbit1993/react-ts.git"//git地址
  },
  "files": ["dist"],//punlish上传的文件
  "main": "dist/index.js",//组件入口
  "module": "dist/index.js",//组件入口
  "types": "dist/index.d.ts",//组件类型入口
  "dependencies": {// 运行依赖
    "@fortawesome/fontawesome-svg-core": "^1.2.35",
    "@fortawesome/free-solid-svg-icons": "^5.15.3",
    "@fortawesome/react-fontawesome": "^0.1.14",
    "axios": "^0.21.1",
    "classnames": "^2.2.6",
    "react-transition-group": "^4.4.2"
  },
  "peerDependencies": {//安装包附加依赖
    "react":"^16.8.0",
    "react-dom":"^16.8.0"
  },
  "scripts": {
    "start": "react-scripts start",
    "clear": "rimraf ./dist",//跨平台清理脚本
    "lint":"eslint --ext js,ts,tsx src --max-warnings 5",//lint规则，校验sr目录下的js,ts,tsx ，最大警告数不超过五个
    "build": "npm run clear && npm run build-ts && npm run build-css",
    "build-ts": "tsc -p tsconfig.build.json",
    "build-css": "node-sass ./src/styles/index.scss ./dist/index.css",//将scss构建成css
    "test": "react-scripts test --coverage --watch",
    "test:nowatch":"cross-env CI=true react-scripts test",//跨平台执行CI脚本
    "eject": "react-scripts eject",
    "storybook": "start-storybook -p 6006 -s public",
    "build-storybook": "build-storybook -s public",
    "prepublish": "npm run test:nowatch && npm run lint && npm run build"//punlish之前的执行钩子
  },
  "eslintConfig": {//lint配置
    "extends": [
      "react-app",
      "react-app/jest"
    ],
    "overrides": [
      {
        "files": [
          "**/*.stories.*"
        ],
        "rules": {
          "import/no-anonymous-default-export": "off"
        }
      }
    ]
  },
  "browserslist": {
    "production": [
      ">0.2%",
      "not dead",
      "not op_mini all"
    ],
    "development": [
      "last 1 chrome version",
      "last 1 firefox version",
      "last 1 safari version"
    ]
  },
  "devDependencies": {
    "react": "^17.0.1",
    "react-dom": "^17.0.1",
    "typescript": "^4.1.3",
    "react-scripts": "4.0.1",
    "web-vitals": "^0.2.4",
    "node-sass": "^4.14.1",
    "@testing-library/jest-dom": "^5.11.8",
    "@testing-library/react": "^11.2.2",
    "@testing-library/user-event": "^12.6.0",
    "@storybook/addon-actions": "^6.3.4",
    "@storybook/addon-essentials": "^6.3.4",
    "@storybook/addon-info": "^5.3.21",
    "@storybook/addon-links": "^6.3.4",
    "@storybook/node-logger": "^6.3.4",
    "@storybook/preset-create-react-app": "^3.2.0",
    "@storybook/react": "^6.3.4",
    "eslint-config-prettier": "^7.1.0",
    "eslint-plugin-prettier": "^3.3.0",
    "husky": "^4.3.6",
    "lint-staged": "^10.5.3",
    "prettier": "^2.2.1",
    "react-docgen-typescript-loader": "^3.7.2",
    "rimraf": "^3.0.2",
    "@types/classnames": "^2.2.11",
    "@types/jest": "^26.0.19",
    "@types/node": "^12.19.11",
    "@types/react": "^16.14.2",
    "@types/react-dom": "^16.9.10",
    "@types/react-transition-group": "^4.4.2",
    "@types/storybook__addon-info": "^5.2.4"
  },
  "husky": {
    "hooks": {//git校验
      "pre-commit": "npm run test:nowatch && npm run lint"//代码提交前校验
      //"pre-commit": "lint-staged" 提交前的操作
    }
  },
  "lint-staged": {
    "*.{js,jsx,ts,tsx}": [
      "prettier --write",//重写
      "eslint --fix",//修复
      "git add"//提交
    ],
    "*.{css,md,scss}": [
      "prettier --write",
      "git add"
    ]
  }
}
```