import time
import os
from PIL import Image, ExifTags
from colorama import init
from colorama import Fore, Back, Style
from datetime import datetime
import imghdr
import mimetypes
import sys

init()
print('')
print(Fore.GREEN +'#####################################################################')
print(Fore.GREEN +'#####################################################################')
print('')
print('           ####      ####   #######  #########   #########')
print('           ##############   ###         ###      ###   ###')
print('           ###  ####  ###   #######     ###      ###   ###')
print('           ###   ##   ###   ###         ###      #########')
print('           ###        ###   #######     ###      ###   ###')
print('')
print('                         -------------------')
print('                        |     CREADTED:     |')
print('                        |    LIZARD ROOT    |')
print('                         -------------------')
print('')
print('                   -------------------------------')
print('                  | https://github.com/LizardRoot |')
print('                  | https://t.me/LizardRoot       |')
print('                   -------------------------------')
print('')
print('         ---------------------------------------------------')
print('        | Make sure that all the pictures you want to check |')
print('        |      are in the same folder as the script.        |')
print('         ---------------------------------------------------')
print('')
print(Fore.GREEN +'#####################################################################')
print(Fore.GREEN +'#####################################################################')
time.sleep(2)
print('')
input('PRESS ENTER TO CONTINUE...')

try:
    os.remove('README.md')
except FileNotFoundError:
    print('')

def metadata(image):
    print('')
    print(Fore.GREEN + '--------------------------------------------------------------------')
    print('')
    
    #Name file 
    print(Fore.GREEN + 'File name:               ' + file)
    if os.path.isdir(image):
        print(Fore.RED + image + ' - is folder. In the folder with the script, leave only the images.')
        input('PRESS ENTER TO EXIT...')
        exit()

    #type file
    type_file = imghdr.what(image)

    if str(type_file) == 'None':
        print(Fore.RED + str(image) + ' - is not an image. Only images should be in the folder.')
        input('PRESS ENTER TO EXIT...')
        exit()

    print(Fore.GREEN + 'Type file:               {type_file}'.format(type_file=type_file))

    #MIME Type
    mime_type = mimetypes.MimeTypes().guess_type(image)[0]
    print(Fore.GREEN + 'MIME type:               {mime_type}'.format(mime_type=mime_type))

    #size 
    file_size_B = os.path.getsize(image)
    file_size_Kb = file_size_B / 1000 
    file_size_MB = file_size_Kb / 1000 
    print(Fore.GREEN + 'File size:               {file_size_Kb} Kb'.format(file_size_Kb=file_size_Kb))
    print(Fore.GREEN + '                         {file_size_MB} MB'.format(file_size_MB=file_size_MB))

    #Image Size
    with Image.open(image) as img:
        width, height = img.size
        print(Fore.GREEN + 'Image size:              {width} x {height}'.format(width=width, height=height))

    #create file 
    create_file_1970 = os.path.getctime(image) # время создания файла
    create_file = datetime.fromtimestamp(create_file_1970).strftime('%Y-%m-%d %H:%M:%S') #конвертация
    print(Fore.GREEN + 'File was created:        {create_file}'.format(create_file=create_file))

    #edit file
    edit_file_1970 = os.path.getmtime(image) # время последнего изменения файла
    edit_file = datetime.fromtimestamp(edit_file_1970).strftime('%Y-%m-%d %H:%M:%S')
    print(Fore.GREEN + 'File has been modified:  {edit_file}'.format(edit_file=edit_file))

    #Make
    img = Image.open(image)
    img_exif = img.getexif()
    if img_exif:
        img_exif_dict = dict(img_exif)
        print(Fore.GREEN + 'Device model:            ' + img_exif_dict[271] + ' ' + img_exif_dict[272])
        print(Fore.GREEN + 'Software:                ' + img_exif_dict[305])
        print(Fore.GREEN + 'Brand of camera:         ' + img_exif_dict[42035])
        print(Fore.GREEN + 'Camera Lens Model:       ' + img_exif_dict[42036])
    else:
        print(Fore.GREEN + 'Device model:            ' + Fore.RED + 'none' )
        print(Fore.GREEN + 'Software:                ' + Fore.RED + 'none' )
        print(Fore.GREEN + 'Brand of camera:         ' + Fore.RED + 'none' )
        print(Fore.GREEN + 'Camera Lens Model:       ' + Fore.RED + 'none' )

    #GPS
    img = Image.open(image)
    img_exif = img.getexif()
    if img_exif:
        img_exif_dict = dict(img_exif)
        lat = img_exif_dict[34853][2][0][0]/1 + (img_exif_dict[34853][2][1][0]/img_exif_dict[34853][2][1][1])/60 + (img_exif_dict[34853][2][2][0]/img_exif_dict[34853][2][2][1])/3600 
        latref = img_exif_dict[34853][1][0]
        if latref == 'S':
            lat = -lat
        lon = img_exif_dict[34853][4][0][0]/1 + (img_exif_dict[34853][4][1][0]/img_exif_dict[34853][4][1][1])/60 + (img_exif_dict[34853][4][2][0]/img_exif_dict[34853][4][2][1])/3600
        lonref = img_exif_dict[34853][3][0]
        if lonref == 'W':
            lon = -lon
        print(Fore.GREEN + 'GPS Latitude:            {lat}'.format(lat=lat))
        print(Fore.GREEN + 'GPS Longitude:           {lon}'.format(lon=lon))
    else:
        print(Fore.GREEN + 'GPS Latitude:            ' + Fore.RED + 'none' )
        print(Fore.GREEN + 'GPS Longitude:           ' + Fore.RED + 'none' )

path = os.path.abspath(os.path.dirname(__file__))
my_dir = os.path.basename(__file__)

for file in os.listdir(path):
    if file == my_dir or file == 'metadata.exe':
        continue
    else:
        metadata(file)
print(Fore.GREEN + '')
input('PRESS ENTER TO EXIT...')
print('')
