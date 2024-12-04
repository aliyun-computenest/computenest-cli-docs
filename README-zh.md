# ComputeNest-CLI

## 项目描述

`computenest-cli` 是一个命令行工具,集成了创建、更新和部署构件,以及在 ComputeNest
框架内创建和更新服务的功能。它允许用户管理他们的服务、构建构件以及处理自定义操作,如自定义镜像创建。

## 要求

- Python >= 3.7

## 安装

可以使用 pip 包管理器安装 `computenest-cli`。

```shell
# 安装 computenest-cli
pip install computenest-cli
```

## 使用方法

要使用 `computenest-cli`，只需运行相应的命令并附带所需的参数。每个命令都带有 `--help` 选项，用于显示有关该命令用法的帮助信息。

### 登录到计算巢

#### 描述

登录到计算巢。

#### 参数

| 参数                    | 必填 | 描述                      | 示例值                    |
|-----------------------|----|-------------------------|------------------------|
| `--access_key_id`     | 否  | 认证所需的访问密钥ID。            | `AKID1234567890`       |
| `--access_key_secret` | 否  | 认证所需的访问密钥密钥。            | `secret1234567890`     |
| `--security_token`    | 否  | 认证所需的Security Token；可选。 | `security_token_value` |

#### 示例：

```bash
computenest-cli login \
    --access_key_id AKID1234567890 \
    --access_key_secret secret1234567890
```

### 列出支持的服务模板

#### 描述

列出支持的服务模板。使用此命令来获取支持的服务模板和项目名称，你可以使用 `computenest-cli init-project --project_name xxx`
来初始化项目。

#### 参数

| 参数               | 必填 | 描述                                     | 示例值     |
|------------------|----|----------------------------------------|---------|
| `--service_type` | 否  | 筛选出指定服务类型，可选值包括 `private` 和 `managed`。 | private |

#### 示例：

```bash
computenest-cli list-projects --service_type private
```

### 初始化项目

#### 描述

通过指定项目名称来初始化项目。使用此命令来下载指定项目到输出路径，随后你可以切换到输出路径中，使用 `computenest-cli import`
来快速创建计算巢服务。

#### 参数

| 参数               | 必填 | 描述             | 示例值               |
|------------------|----|----------------|-------------------|
| `--project_name` | 是  | 项目名称。          | `MyProject`       |
| `--output_path`  | 是  | 项目文件保存的输出目录路径。 | `/path/to/output` |

#### 示例：

```bash
computenest-cli init-project --project_name springboot-package-ecs-demo --output_path .
```

### 导入服务

#### 描述

通过导入配置文件来创建或更新服务。

#### 参数

| 参数                    | 是否必需 | 描述                                                         | 示例值                                    |
|-----------------------|------|------------------------------------------------------------|----------------------------------------|
| `--access_key_id`     | 否    | 认证所需的访问密钥ID。使用login命令登录后无需使用，若指定则以该命令的access_key_id为准。     | `AKID1234567890`                       |
| `--access_key_secret` | 否    | 认证所需的访问密钥密钥。使用login命令登录后无需使用，若指定则以该命令的access_key_secret为准。 | `secret1234567890`                     |
| `--file_path`         | 是    | 导入过程中所需文件的路径。                                              | `/path/to/your/config.yaml`            |
| `--region_id`         | 否    | 服务所在区域的ID。                                                 | `cn-hangzhou`                          |
| `--service_id`        | 否    | 服务的唯一标识符。如提供，命令将尝试根据此`service_id`搜索服务。                     | `service-12345`                        |
| `--service_name`      | 否    | 服务名称。如果不提供`service_id`，将使用此名称来匹配服务。                        | `my-service`                           |
| `--version_name`      | 否    | 服务的特定版本名称。                                                 | `v1.0`                                 |
| `--desc`              | 否    | 服务的简短描述。                                                   | `示例服务`                                 |
| `--update_artifact`   | 否    | 指定是否需要更新制品。接受`True`或`False`。                               | `True`                                 |
| `--icon`              | 否    | 存储在OSS（对象存储服务）上的服务自定义图标的URL。如不提供则直接使用仓库中默认图标。              | `https://xxx/icon.png`                 |
| `--security_token`    | 否    | 认证所需的Security Token；可选。                                    | `security_token_value`                 |
| `--parameters`        | 否    | 以字符串形式的JSON格式参数。可选，默认为空的JSON对象`{}`。                        | `{"key1": "value1", "key2": "value2"}` |
| `--parameter_path`    | 否    | 参数文件的路径。如提供，这将覆盖`--parameters`选项。                          | `/path/to/parameters.json`             |

注意：

- 如果提供了 --service_id，命令将尝试唯一匹配该 ID，并且服务名称可以被修改。如果未找到，将引发 ServiceNotFound 错误。
- 如果没有提供 --service_id，但提供了 --service_name，则命令将基于服务名称进行匹配。

#### 示例

```bash
computenest-cli import \
    --region_id cn-hangzhou \
    --update_artifact True \
    --service_id service-12345 \
    --service_name my-service \
    --version_name v1.0 \
    --icon https://xxx/icon.png \
    --desc "Sample service" \
    --file_path /path/to/your/config.yaml \
    --parameter_path /path/to/parameters.json
```

将 `$ACCESS_KEY_ID` 和 `$ACCESS_KEY_SECRET` 分别替换为您的 AccessKey ID 和 AccessKey Secret。
如何获取 AccessKey 对： https://help.aliyun.com/zh/id-verification/cloudauth/obtain-an-accesskey-pair

### 导出服务

#### 描述

通过指定服务 ID 和输出目录来导出服务。

#### 参数

| 参数                      | 必填 | 描述                                                                         | 示例值                    |
|-------------------------|----|----------------------------------------------------------------------------|------------------------|
| `--region_id`           | 是  | 服务所在区域的 ID。                                                                | `cn-hangzhou`          |
| `--service_id`          | 是  | 服务的唯一标识符。                                                                  | `service-12345`        |
| `--output_dir`          | 是  | 导出文件保存的输出目录路径。                                                             | `/path/to/output`      |
| `--version_name`        | 否  | 服务的具体版本名称。                                                                 | `v1.0`                 |
| `--export_type`         | 否  | 导出类型。选项包括 `CONFIG_ONLY` 和 `FULL_SERVICE`。                                  | `config_only`          |
| `--export_project_name` | 否  | 被导出的项目名称。                                                                  | `MyExportedProject`    |
| `--export_file_name`    | 否  | 导出的配置文件名称；默认为 `config.yaml`。                                               | `my_config.yaml`       |
| `--access_key_id`       | 是  | 进行身份验证所需的 Access Key ID。使用login命令登录后无需使用，若指定则以该命令的access_key_id为准。         | `AKID1234567890`       |
| `--access_key_secret`   | 是  | 进行身份验证所需的 Access Key Secret。使用login命令登录后无需使用，若指定则以该命令的access_key_secret为准。 | `secret1234567890`     |
| `--security_token`      | 否  | 身份验证所需的安全令牌；可选。                                                            | `security_token_value` |

#### 示例：

```bash
computenest-cli export \
    --region_id cn-hangzhou \
    --service_id service-12345 \
    --version_name v1.0 \
    --export_type config_only \
    --output_dir /path/to/output \
    --export_project_name MyExportedProject \
    --export_file_name my_config.yaml
```

### 生成文件或项目

#### 描述

通过指定输出目录生成文件或项目。

#### 参数

| 参数                  | 必填 | 描述                                       | 示例值                        |
|---------------------|----|------------------------------------------|----------------------------|
| `--output_path`     | 是  | 生成文件保存的输出目录路径。                           | `/path/to/output`          |
| `--file_path`       | 否  | 要生成的具体文件的路径。                             | `/path/to/file.txt`        |
| `--type`            | 否  | 生成类型。选项包括 `file`（单个文件）或 `project`（整个项目）。 | `file`                     |
| `--parameters`      | 否  | JSON 格式的参数字符串。默认为空的 JSON 对象 `{}`。        | `{"key1": "value1"}`       |
| `--parameter_path`  | 否  | 参数文件的路径。如果提供，将覆盖 `--parameters` 选项。      | `/path/to/parameters.json` |
| `--overwrite`, `-y` | 否  | 如果设置，则确认在不提示的情况下覆盖输出文件。                  | （无示例，仅为标志）                 |

#### 示例：

```bash
computenest-cli generate \
    --type file \
    --file_path /path/to/file.txt \
    --parameters '{"key1": "value1", "key2": "value2"}' \
    --output_path /path/to/output \
    --parameter_path /path/to/parameters.json \
    --overwrite
```

### 获取帮助

要获取特定命令的帮助，请在命令后添加 `--help`：

```shell
computenest-cli import --help
```

## 如何获取 AccessKey 对

按照以下说明创建 AccessKey
对：[创建 AccessKey 对](https://www.alibabacloud.com/help/en/ram/user-guide/create-an-accesskey-pair)

