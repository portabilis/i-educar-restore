# i-educar-restore

Este é um script para auxiliar no download e na restuaração de bases para o
sistema i-Educar. Para que ele funcione são feitas algumas suposições:

- Os backups são guardados no serviço da AWS S3;
- É feito um backup diário da base.

## Como instalar

...

## Quick start

```
$ restore --help
Database restore tool for i-Educar.

OPTIONS
  --aws-bucket          AWS bucket (can be informed in the config file).
  --aws-key             AWS key (can be informed in the config file).
  --aws-prefix          AWS search prefix (can be informed in the config file).
  --aws-region          AWS prefered region (can be informed in the config
                        file).
  --aws-secret          AWS secret (can be informed in the config file).
  --base, -b            Database name to be restored. Ex.: saopaulo.
  --config-file, -c     Config file path. Defaults to "config.ini" in the
                        current directory.
  --date, -d            Backup date. Defaults to current date.
  --help, -?            Display this help.
  --host, -h            DB host (can be informed in the config file).
  --include-audit, -i   Restore with audit tables. They are not restored by
                        default.
  --newbase, -B         New database to be created. Defaults to --base.
  --password, -P        DB password (can be informed in the config file).
  --port, -p            DB port (can be informed in the config file).
  --user, -u            DB user (can be informed in the config file).

$ restore -b ieducar
```

## Configurações

Em geral as chaves da configuração são auto explicativas mas algumas
configurações tem comportamento especial:

| Chave | Descrição |
|---|---|
| date| Por padrão o script irá buscar por backups na data atual mas você pode informar alguma outra data com esta chave. Ex. `--date 2019-01-01` |
| aws-prefix | Quando o sistema for buscar pelo arquivo de backup no S3 ele irá usar esta chave como parâmetro para a busca. Por se tratar de um valor dinâmico você pode usar placeholders que serão substituidos pelos valores de outras chaves da configuração. Ex.: `--aws-prefix backup/ieducar_{date}`. Neste exemplo, na data 12/12/2018 o prefixo será `backup/ieducar_2018-12-12`. |
| newbase | Por padrão o sistema irá baixar a base sendo buscada e irá restaurar no banco de dados com o mesmo nome. Isso quer dizer que se o banco já existir ele será removido. Se quiser evitar a remoção do banco você pode informar um nome alternativo para a nova base. Ex.: `--newbase nova_restauracao`. A exemplo da chave `aws-prefix` você também pode usar placeholders no nome da nova base. |

Além das chaves de configuração informadas acima, internamente você também tem acesso às seguintes chaves para serem usadas como placeholders:

- `year`
- `month`
- `day`

## Bulding

Para gerar um arquivo *.phar a partir do repositório e recomendado usar o
[Box](https://box-project.github.io/box2/).

Após instalar o Box (via phar ou como um pacote composer global), basta rodar o
seguinte comando na raíz do projeto:

```
$ box build
```