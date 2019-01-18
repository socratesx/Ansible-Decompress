# Ansible-Decompress

Ansible module for decompressing unarchived compressed files such as filename.iso.gz

<h3>Description</h3>

The module gets either a gz,bz2 or zip compressed file and just uncompress its content. Its purpose is to cover the case where a single file is just compressed such as a bootimage.iso.gz. but not archived. In case the file is archived, e.g. bootimage.tar.gz then the core module unarchive can handle it.

<b>Parameters</b>
  
<b>src:</b>	
<br>This is the absolute path of the compressed file. This parameter is required.
	
<b>dst:</b> 	
<br>The destination of the uncompressed file. This can be an absolute file path where the content will be saved as the specified filename, a directory where the filename will match the src filename without the (.gz|.bz2|.zip) extension or undefined. In the last case the file will be uncompressed in the same directory of the src with the same filename without the compress extension. This parameter is not required.
	
<b>force:</b> 	
<br>Set this option to True to overwrite the extracted file in case it exists on destination. The parameter is not required and defaults to False. 
    	
<b>update:</b>	
<br>Set this to True to overwrite the file only if it has different size. This parameter is not required and defaults to True. It is only relevant with zip files.

<h3>Examples</h3>

This will decompress the bootimage.iso.gz and move it to ~/isos/bootimage.iso

	name: Decompress a gzipped iso image downloaded from the Internet
  	decompress:
      src: '/tmp/bootimage.iso.gz'
      dest: '~/isos/'

Decompress the file as newname_image.iso

	name: Decompress a bz2 iso image and extract as newname.iso in ~/isos/
  	decompress:
      src: '/tmp/bootimage.iso.bz2'
      dest: '~/isos/newname_image.iso'

Decompress multiple images using with_items

	name: decompress multiple images
    decompress:
      src: "{{ item }}"
      dest: '/my-images/'
      force: true
  	with_items: "{{ compressed_isos_list }}"

Setting just the src will use the same name omitting the extension, the result will be /tmp/bootimage.iso

	name: Decompress in the same folder using the same name
  	decompress:
      src: '/tmp/bootimage.iso.zip'

<h3>How to Use</h3>
Just add it in your role's library folder or the ansible module folder and call it just as any ansible module. The module requires python version 2.7 on the target host.
