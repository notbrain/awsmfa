## Installation

```
npm i -g @notbrain/awsmfa (COMING SOON)
```

```
git clone --depth 1 https://github.com/notbrain/awsmfa
cd awsmfa
npm i -g
```

## Requirements

Your aws credentials should be located at `~/.aws/credentials` and the file should look something like the one below. You can't have a `[default]` profile because that will be overwritten by this program. `mfa_arn` is the arn of the mfa device that you've registered with a particular IAM user.

```
[profile-1]
aws_access_key_id = AKLFJEIFJAELSEKFJASDLKFJ
aws_secret_access_key = lsafjio3jlk2j4kl2j4j23lk4j23kl4j12
mfa_arn = arn:aws:iam::lsfaiselfjlaskjkda:mfa/profile-1

[profile-2]
aws_access_key_id = LAKSJLAFEIFJALJKFKSDJF
aws_secret_access_key = aeijei02jfeji2j0fijijf20ijflvklkl
mfa_arn = arn:aws:iam::lsfaiselfjlaskjkda:mfa/profile-2
```

## Usage

After installation you should have access to the `awsmfa` command. The first argument is the profile you want to use and the second argument is the MFA token.

**Example**

`awsmfa profile-1 382973`

The commmand adds a `[default]` profile to your credentials file with a session-token. The session is valid for 24 hours. The credentials file would now look something like this.

```
[profile-1]
aws_access_key_id = AKLFJEIFJAELSEKFJASDLKFJ
aws_secret_access_key = lsafjio3jlk2j4kl2j4j23lk4j23kl4j12
mfa_arn = arn:aws:iam::lsfaiselfjlaskjkda:mfa/profile-1

[profile-2]
aws_access_key_id = LAKSJLAFEIFJALJKFKSDJF
aws_secret_access_key = aeijei02jfeji2j0fijijf20ijflvklkl
mfa_arn = arn:aws:iam::lsfaiselfjlaskjkda:mfa/profile-2

[default]
aws_access_key_id = ASIAIORDHLIN536UOYLQ
aws_secret_access_key = hc5mLgs8mFP5KVN5js+nHdTJqkkxLRSiCJeMP0kR
aws_session_token = FQoDYXdzENj//////////wEaDBSRyXR9SFAmXO/r/yKwAVDM2JNEGzIT4xAcFWRjOviFWZvKsTr6IXi2gXwvRnGIHHYDLwp89m0CsKMmsR+olGfnUJCd8LoD9M5ckCPIZcu5tsSNibR/wVJV4Rnnhksw+ZXKs8vCzN28/0EWCKPorIBFyvb6TWHyEx6mko2YeZNrS+dJfG3j4Ss5M1jGo9x1tmavp4HW4dtVnqKwh0cJYnpJ6zur6XMIG9jlRsp+1LWg/cQrwVdHiPrN8Lca8PX2KLCr+dUF
```

Now, when you use the AWS CLI, it defaults to using the session you last started. If you think what this package offers is too simplistic and you want more control, check out [broamski/aws-mfa](https://github.com/broamski/aws-mfa).

## Shell Variable Export Option

If you set `AWSMFA_CACHE_ENABLED=true`, awsmfa will create a file named whatever the value of `AWSMFA_TIMEOUT_FILE` is. For example if you set:

```
AWSMFA_CACHE_ENABLED=true|false
AWSMFA_TIMEOUT_FILE="$HOME/.awsmfa/timeout"
```

The file `$HOME/.awsmfa/timeout` will contain

```
export AWSMFA_EXPIRE_EPOCH=<epoch_of_expiration>
export AWSMFA_PROFILE=<profile_name>
```

which can be used to calculate the expiration time and current profile active for use in custom shell prompts.
