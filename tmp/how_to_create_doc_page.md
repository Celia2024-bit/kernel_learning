kernel_learning/                          ← repo 根目录
├── mkdocs.yml                            ← 放在根目录(刚生成的第一个文件)
├── requirements.txt                      ← 放在根目录(第二个文件)
├── .github/
│   └── workflows/
│       └── deploy-docs.yml               ← 建这两层文件夹,把第三个文件放进去
├── README.md
└── docs/
    ├── index.md                          ← 新建,内容抄 README.md
    ├── images/                           ← 图片挪到这里面
    │   └── bootloader_kernel_handover.png
    ├── Low-Latency1_Why_context_switches_matter.md
    ├── Low-Latency2_What_is_a_context_switches.md
    ├── Low-Latency3_What-cause-a-page-fault.md
    ├── Low-Latency4_Practical-low-latency-playbook.md
    ├── Low-Latency5_Phased-implementation-checklist.md
    └── Bootloader and Kernel Handover.md

```shell
pip install -r requirements.txt
mkdocs serve
# Open http://127.0.0.1:8000
```

Open http://127.0.0.1:8000

or 

```shell
mkdocs serve -a 127.0.0.1:8080
# open http://127.0.0.1:8080
```

https://celia2024-bit.github.io/kernel_learning/



* create an empty file doc.nojekyll  (keep the same style as the local )

```git
git add docs/.nojekyll
git commit -m "fix: add .nojekyll to bypass Jekyll processing"
git push origin main
```

- workflow permisson 
  
  ```textile
  Settings-> Actions->General->Workflow permissions->Read and write permissions->save
  ```

- Delpoy a page 
  
  ```context
  Settings->Pages->Build and deployment->Source 
  Choose 
  Deploy from a branch
  Branch -> gh-pages    /root  
  ```


post docs in https://dev.to/celia2024bit