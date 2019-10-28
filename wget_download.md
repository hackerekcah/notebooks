# Multi-thread downloading

```
# wget_thread.py

import os
import threading
from itertools import product


class Downloder():
    def download_manager(self, url, destination='Files/DownloderApp/', try_number="10", time_out="60"):
        threading.Thread(target=self._wget_dl, args=(url, destination, try_number, time_out)).start()

    def _wget_dl(self,url, destination, try_number, time_out):
        import subprocess
        command=["proxychains", "wget", "-c", "-P", destination, "-t", try_number, "-T", time_out , url]
        try:
            download_state=subprocess.call(command)
        except Exception as e:
            print(e)

        return download_state


if __name__ == '__main__':
   
    modalities = ['audio', 'image']
    input_reprs = ['linear', 'mel128', 'mel256']
    content_type = ['music', 'env']
    model_version_str = 'v0_2_0'
    weight_files = ['openl3_{}_{}_{}.h5.gz'.format(*tup)
        for tup in product(modalities, input_reprs, content_type)]
    base_url = 'https://github.com/marl/openl3/raw/models/'
    dest_path = '/data/songhongwei/Desktop/L3_2/'
    d = Downloder()
    for weight_file in weight_files:
        weight_path = os.path.join(base_url, weight_file)
        print(weight_path)
        d.download_manager(url=weight_path, destination=dest_path)

```
