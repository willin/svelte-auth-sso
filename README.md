![Logo](https://repository-images.githubusercontent.com/726691357/f09bf6fc-3844-4584-8eee-6bfb425d8a38)

# svelte-auth-sso strategy

The SSO strategy is used to authenticate users against a SSO account. It extends the OAuth2Strategy.

## Supported runtimes

| Runtime    | Has Support |
| ---------- | ----------- |
| Node.js    | ✅          |
| Cloudflare | ✅          |
| Vercel     | ✅          |

## Usage

### Create an OAuth application

Follow the steps on [the SSO documentation](https://github.com/willin/sso) to create a new application and get a client ID and secret.

### Create the strategy instance

```ts
import { SSOStrategy } from '@svelte-dev/auth-sso';

let ssoStrategy = new SSOStrategy(
  {
    clientID: 'YOUR_CLIENT_ID',
    clientSecret: 'YOUR_CLIENT_SECRET',
    callbackURL: 'https://example.com/auth/sso/callback'
  },
  async ({ accessToken, extraParams, profile }) => {
    // Get the user data from your DB or API using the tokens and profile
    return profile;
  }
);

authenticator.use(ssoStrategy);
```

### Setup your routes

```tsx
export default function Login() {
  return (
    <form action='/auth/sso' method='post'>
      <button>Login with SSO</button>
    </form>
  );
}
```

```tsx
// routes/auth/sso/+server
import { authenticator } from '~/auth.server';
import type { RequestHandler } from './$types';

export const POST: RequestHandler = async (event) => {
  return authenticator.authenticate('sso', event);
};
```

```tsx
// routes/auth/sso/callback/+server
import { authenticator } from '~/auth.server';
import type { PageServerLoad } from './$types';

export const load: PageServerLoad = async ({ event }) => {
  return authenticator.authenticate('sso', event, {
    successRedirect: '/dashboard',
    failureRedirect: '/login'
  });
};
```

## 赞助 Sponsor

维护者 Owner： [Willin Wang](https://willin.wang)

如果您对本项目感兴趣，可以通过以下方式支持我：

- 关注我的 Github 账号：[@willin](https://github.com/willin) [![github](https://img.shields.io/github/followers/willin.svg?style=social&label=Followers)](https://github.com/willin)
- 参与 [爱发电](https://afdian.net/@willin) 计划
- 支付宝或微信[扫码打赏](https://user-images.githubusercontent.com/1890238/89126156-0f3eeb80-d516-11ea-9046-5a3a5d59b86b.png)

Donation ways:

- Github: <https://github.com/sponsors/willin>
- Paypal: <https://paypal.me/willinwang>
- Alipay or Wechat Pay: [QRCode](https://user-images.githubusercontent.com/1890238/89126156-0f3eeb80-d516-11ea-9046-5a3a5d59b86b.png)

## 许可证 License

Apache-2.0
