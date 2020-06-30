# sdf
 - zoom.exe  찾아보기 혹은 만들기??
 - 
 - 
# day2 1교시
>
  - 파이썬에서 일부 파트를 지칭 : 모듈
  - 파이썬에서는 패키지 모듈 동일한것을 지칭함, pygame이라는 라이브러리 패키지를 사용해서 쉽게만들수 있다!
    ```py
    import pygame as pg
    pg.init()
    pg.mixer.music.load('kangnam.mp3')
    pg.mixer.music.play()#소문자는 매서드
    clock = pg.time.Clock()#대문자는 생성자, 클락 객체 생성
    while pg.mixer.music.get_busy():#여기서 계속 폴링 체크(믹서가 플레이) 
    # 믹서가 아직도 바쁜지 체크, 음악파일을 던져주고 계속 출력중인데,  
        clock.tick(30)#클락은 시스템 틱타임, 30틱만큼 시간소요
        ## 틱은 커널에서 돌고있는 가장 작은 시간단위 ms 아님, 가변적임. 다소 짧은 시간
        ## 파이게임 모듈로 MP3 만들수 있음!!!   
    ```
  - 좋은코드가 아니다! : 비지를 계속 폴링하고 있어서
  - clock.tick(30) : 상품으로 갈때는 이런코드 쓰면 안됨, 쓰루풋 멀티태스킹 
  - 멀티태스킹 : RTOS, 리눅스커널, 
  - 멀티태스킹의 원리는 >> ROTS, 커널에서는 
  - RTOS, 리눅스에서 서로 다르다 뭐가? : 쓰레드, 큐, 프로세스, 세마포어 편향적인 생각하지 말고 둘다 듣기를 권한다!! (KEA에서 강의) 
  - 
    ```py
    import pygame as pg

    def play_music(music_file, volume):
        # 코멘트 트릭, 원래는 한줄코맨트만 지원하지만 여러줄커맨드 트릭 '''
        '''
        stream music with mixer.music module in a blocking manner
        this will stream the sound from disk while playing
        '''
        # set up the mixer 믹서 셋업, 프리퀀시, 비트레이트, 체널, 버퍼
        freq = 44100     # audio CD quality
        bitsize = -16    # unsigned 16 bit 
        # 이상하다 -16비트? 알사? 부르거나 했을꺼야 분석을하다보면 알사나 다른 래핑된 라이브러리가 있었을꺼야
        # 틀릴수 있다 그냥 배우고 넘어가라, 커널쪽으로 갈때는 이값이 아니지만 사용자 인터페이스에서는 의도에 따라서 값이 바뀔수 있으니 대애애애애충 이해하고 넘어가라
        # mplay : 알사를 씀
        # omxplayer : 알사를 안씀
        # 오엠엑스 플레이어는 알사쓴게 아니다!! 엠플레이어는 알사를 직접 쓴거다. 어떤 라이브러리로 구현했나 틀릴수 있다.

        channels = 2     # 1 is mono, 2 is stereo
        buffer = 2048    # number of samples (experiment to get best sound)
        pg.mixer.init(freq, bitsize, channels, buffer)#내부적으로 PCM을 셋팅한다
        # volume value 0.0 to 1.0 
        # v파이게임은 볼륨을 0~1 실수값으로 처리한다
        pg.mixer.music.set_volume(volume)
        clock = pg.time.Clock()
        #try except 문으로 에러 처리 한다!!!
        try:
            pg.mixer.music.load(music_file)
            print("Music file {} loaded!".format(music_file))
        except pg.error:
            print("File {} not found! ({})".format(music_file, pg.get_error()))
            return
        pg.mixer.music.play()
        while pg.mixer.music.get_busy():
            # check if playback has finished
            clock.tick(30)
    # =================================================
    # 파이썬에서 메인코드임
    # pick a MP3 music file you have in the working folder
    # otherwise give the full file path
    # (try other sound file formats too)
    music_file = "kangnam.mp3"

    # optional volume 0 to 1.0
    volume = 0.6

    play_music(music_file, volume)
    # 시스템볼륨에 연동되는걸 보니 ALSA로 구현됬을듯
    # OMX 일수도 있으니 결국 까봐야 된다.. (무슨,,,?? 그래서 뭐지)
    # 라즈베리파이 미디어 프레임웍은 세계표준 : 오픈맥스
    # OpenMAX : 크로노스그룹에서 오픈맥스로 가라고 임베디드는 몰고 있음
    # 어플리케이션 
    # OMXPlayer 매뉴얼을 뒤져보면 시스템볼륨이랑 연동되는 기능도 있다.
    # 그래서 OpenMAX 로 OMXPlayer로 연동이 되어있다
    ```
    - 파이게임만 있는게 아니다 멀티미디어 프로그래밍할수 있는거 짱많다
    - 파이썬으로 멀티미디어 제어 만드는게 최고다! 
    - 예를들면 쥐스트리머, 오엠엑스도 영상출력이 된다.
    - 쥐스트리머를 써서 코딩하려면 공부를 상당히 많이해야되는데, 파이께임같은 패키지 쓰면 직관적으로 프레임워크 몰라도 그냥 가져다가 쓸 수 있다.
    - 여러분이 어느규모로 어플리케이션을 만드느냐에 따라서 선택한다 간단한 응용을 써서 미디어 한다고 하면 파이썬 패키지 검색해서 가져다가 써라!!!!
    - 파이썬은 디폴트로 배우는게 좋아요(AI, )
    > 뭐 만들고싶으면 파이썬패키지로 이것저것 가져다가 써라
    ## 이미지와 영상처리
    > 멀티미디어: 오감을 컴퓨터세계에 구현 , 하지만 지금은 오디오 비디오(청각과 시각)만을 구현하고 있음
    - 멀티미디어 표준 라이브러리 : 
    - 
    ## 설명정리
    > 설명정리
    - 실습, 프레임버퍼 디바이스 드라이버 파일 읽어서 시스템프로그래밍
      - gcc fbinfo.c
      - 
      ```c++
            
        #include <stdio.h>
        #include <stdlib.h>
        #include <fcntl.h>
        #include <sys/ioctl.h>
        #include <linux/fb.h>

        int main(int argc, char *argv[])
        {
                int fbfd = -1;
                struct fb_var_screeninfo vinfo, n_vinfo;
                struct fb_fix_screeninfo finfo;

                int xres=0, yres=0;
                if(argc > 1) {
                        xres = atoi(argv[1]);
                        yres = atoi(argv[2]);
                }

                fbfd = open("/dev/fb0", O_RDWR);
                if(fbfd < 0 ) {
                        perror("Frame Buffer File Open Error");
                        exit(-1);
                }
                printf("fb device open ok!!\n");

                if(ioctl(fbfd, FBIOGET_FSCREENINFO, &finfo) < 0) {
                        perror("Frame Buffer Fixed Info Read Error");
                        exit(-1);
                }

                if(ioctl(fbfd, FBIOGET_VSCREENINFO, &vinfo) < 0) {
                        perror("Frame Buffer Variable Info Read Error");
                        exit(-1);
                }

                printf("Resolution: %d x %d, %d bpp\n", vinfo.xres, vinfo.yres,
                                        vinfo.bits_per_pixel);
                printf("Virtual Resolution: %d x %d\n", vinfo.xres_virtual, vinfo.yres_virtual);
                printf("Length of frame buffer memory: %d\n", finfo.smem_len);

        #ifdef TEST
                //origin:800x480
                if(xres==0) vinfo.xres = 800;
                else vinfo.xres = xres;
                if(yres==0) vinfo.yres = 480;
                else vinfo.yres = yres;
                if(ioctl(fbfd, FBIOPUT_VSCREENINFO, &vinfo) < 0) {
                        perror("Frame Buffer Variable Info Set Error");
                        exit(-1);
                }

                if(ioctl(fbfd, FBIOGET_VSCREENINFO, &n_vinfo) < 0) {
                        perror("Frame Buffer Variable Info Read Error");
                        exit(-1);
                }
                printf("\n==>New Resolution: %d x %d, %d bpp\n", n_vinfo.xres, n_vinfo.yres,
                                        n_vinfo.bits_per_pixel);
        #endif
                close(fbfd);

                return 0;
        }


      ```
      - 결과 
      ```
      
        fb device open ok!!
        Resolution: 1184 x 624, 32 bpp
        Virtual Resolution: 1184 x 624
        Length of frame buffer memory: 2955264

      ```
    - gcc fbinfo.c -DTEST 
    - DTEST 옵션으로 테스트 디파인을 주면 바꿀쑤 있음 
      - ./a.out
    - df -h : -h 옵션이 인간이 쉽게 
    - 이미지를 구울떄 8기가로 인정해서, 있는걸 못쓰는거에요, 이걸 체크포인트
    - 라즈베리파이 컨피그에서 조종할꺼야



- 커널공부 정답이 없다.
- 커널공부를 바라보는 관점  
  - 직접주소


# day2 2교시
>
  - vcgencmd commands : 하드웨어 볼수 있는 테스트 용도로 쓰는 명령어
    - https://www.raspberrypi.org/documentation/raspbian/applications/vcgencmd.md
  - 카메라쪽 : vcgencmd get-camera  

  - 카메라 테스트
  ```
  pi@raspberrypi:~ $ vcgencmd get_camera
  supported=1 detected=1
  ```
  - sudo raspistill -o tset : 되는데...?
  - 안드로이드도 지금 갤럭시 20 이런것도 리눅스커널 2점대랑 3점대 쓴다....
  - sudo modprobe bcm2835-v4l2  : 사실은 bcm2837인데 카메라는 변경점이 없다..
    - 파워끄면 모듈 날라가 -> 자동들어오게 하고싶음 루트및에 폴더에다가 짱박아둬라
    - 있는지 확인
      ```
      lsmod | grep v4l2
      ```
  ```
  pi@raspberrypi:~ $ lsmod | grep v4l2
  ```
  ```
  bcm2835_v4l2           53248  0
  v4l2_common            16384  1 bcm2835_v4l2
  videobuf2_vmalloc      16384  1 bcm2835_v4l2
  videobuf2_v4l2         24576  1 bcm2835_v4l2
  videobuf2_core         45056  2 bcm2835_v4l2,videobuf2_v4l2
  videodev              184320  4 v4l2_common,videobuf2_core,bcm2835_v4l2,videobuf2_v4l2
  ```
  - pi@raspberrypi:~ $ ls -l /dev/vide*
    ```
    crw-rw----+ 1 root video 81, 0 Jun 30 11:03 /dev/video0
    ```
  - 욕토 : 요세 많이 쓰는데, 수요가 매우 한정적, 점점 늘어나긴 하는데,, 많이 늘어나면 다룰순 있다.
    - 워낙 편해서 욕토쪽으로 많이 가려고 하더라구요 (자동차 현대오트론 이런데서는 욕토기반..)
  - 요즘 대세는 임베디드 관심X 교육 열어도 안와 트렌드 많이탄다. 임베디드 떳을때는 어마어마하게 커널강의 많이했지만, 죄다 AI로 가있다..
  - 요즘에는 AI다 대세는 AI , 빅데이터 AI
  - pi@raspberrypi:~/iot-mm/ch02/02.fb_image $ cat point.c
  ```c
    #include <stdio.h>
    #include <stdlib.h>
    #include <fcntl.h>
    #include <sys/ioctl.h>
    #include <linux/fb.h>

    #ifdef BPP16
    unsigned short makepixel(unsigned char r, unsigned char g, unsigned char b)
    {
            return (unsigned short)( ((r>>3)<<11) | ((g>>2)<<5) | (b>>3) );
    }
    #else
    unsigned int makepixel(unsigned char r, unsigned char g, unsigned char b)
    {
            return (unsigned int)( (r<<16) | (g<<8) | (b) );//지금은 ARGB라는 구조를 알수있다
    }
    #endif

    #ifdef BPP16 // 16비트시절에
    static int DrawPoint(int fd, int x, int y, unsigned short color)
    #else  // 지금은 32비트
    static int DrawPoint(int fd, int x, int y, unsigned int color)
    #endif
    {
            int offset;
            struct fb_var_screeninfo vinfo;

            if(ioctl(fd, FBIOGET_VSCREENINFO, &vinfo) < 0) {
                    perror("Frame Buffer Variable Info Read Error");
                    exit(-1);
            }

    #ifdef BPP16
            offset = (x + y*vinfo.xres)*2;
            lseek(fd, offset, SEEK_SET);
            write(fd, &color, 2);
    #else
            offset = (x + y*vinfo.xres)*4;//해상도에다가 4를 곱해서
            lseek(fd, offset, SEEK_SET);
            write(fd, &color, 4);//프레임버퍼에다가 직접 써버리는 무식한 이그잼플이다!!!
    #endif
            return 0;
    }

    int main(int argc, char *argv[])
    {
            int fbfd, status, offset;
            unsigned short pixel;

            fbfd = open("/dev/fb0", O_RDWR);
            if(fbfd < 0 ) {
                    perror("Frame Buffer File Open Error");
                    exit(-1);
            }
            printf("fb device open ok!!\n");

            DrawPoint(fbfd, 100, 350, makepixel(255, 0, 0));
            DrawPoint(fbfd, 100, 351, makepixel(255, 0, 0));
            DrawPoint(fbfd, 100, 352, makepixel(255, 0, 0));
            DrawPoint(fbfd, 200, 350, makepixel(0, 255, 0));
            DrawPoint(fbfd, 200, 351, makepixel(0, 255, 0));
            DrawPoint(fbfd, 200, 352, makepixel(0, 255, 0));
            DrawPoint(fbfd, 300, 350, makepixel(0, 0, 255));
            DrawPoint(fbfd, 300, 351, makepixel(0, 0, 255));
            DrawPoint(fbfd, 300, 352, makepixel(0, 0, 255));

            close(fbfd);

            return 0;
    }
  ```
  - man lseek
  - 두가지 인클루드 추가해야함
  ```
    #include <sys/types.h>
    #include <unistd.h>
  ```
  - 프레임버퍼에다가 이렇게 찍어버리면 하드웨어에 맵핑시키는거야!!! ㅎㄷㄷ
  - 다음예제 라인그리기
    - 라인그리는 코드
    pi@raspberrypi:~/iot-mm/ch02/02.fb_image $ cat line.c
    ```cpp
        
    #include <stdio.h>
    #include <stdlib.h>
    #include <fcntl.h>
    #include <sys/ioctl.h>
    #include <linux/fb.h>

    #ifdef BPP16
    unsigned short makepixel(unsigned char r, unsigned char g, unsigned char b)
    {
            return (unsigned short)( ((r>>3)<<11) | ((g>>2)<<5) | (b>>3) );
    }
    #else
    unsigned int makepixel(unsigned char r, unsigned char g, unsigned char b)
    {
            return (unsigned int)( (r<<16) | (g<<8) | (b) );
    }
    #endif

    #ifdef BPP16
    static int DrawPoint(int fd, int x, int y, unsigned short color)
    #else
    static int DrawPoint(int fd, int x, int y, unsigned int color)
    #endif
    {
            int offset;
            struct fb_var_screeninfo vinfo;

            if(ioctl(fd, FBIOGET_VSCREENINFO, &vinfo) < 0) {
                    perror("Frame Buffer Variable Info Read Error");
                    exit(-1);
            }

    #ifdef BPP16
            offset = (x + y*vinfo.xres)*2;
            lseek(fd, offset, SEEK_SET);
            write(fd, &color, 2);
    #else
            offset = (x + y*vinfo.xres)*4;
            lseek(fd, offset, SEEK_SET);
            write(fd, &color, 4);
    #endif
            return 0;
    }

    #ifdef BPP16
    static int DrawLine(int fd, int start_x, int end_x, int y, unsigned short color)
    #else
    static int DrawLine(int fd, int start_x, int end_x, int y, unsigned int color)
    #endif
    {
            int x, offset;
            struct fb_var_screeninfo vinfo;

            if(ioctl(fd, FBIOGET_VSCREENINFO, &vinfo) < 0) {
                    perror("Frame Buffer Variable Info Read Error");
                    exit(-1);
            }

            for(x=start_x; x<end_x; x++) {
    #ifdef BPP16
                    offset = (x + y*vinfo.xres)*2;
                    lseek(fd, offset, SEEK_SET);
                    write(fd, &color, 2);
    #else
                    offset = (x + y*vinfo.xres)*4;
                    lseek(fd, offset, SEEK_SET);
                    write(fd, &color, 4);
    #endif
            }

            return 0;
    }

    int main(int argc, char *argv[])
    {
            int fbfd, status, offset;
            unsigned short pixel;

            fbfd = open("/dev/fb0", O_RDWR);
            if(fbfd < 0 ) {
                    perror("Frame Buffer File Open Error");
                    exit(-1);
            }
            printf("fb device open ok!!\n");

            DrawLine(fbfd, 0, 100, 200, makepixel(0, 0, 255));
            DrawLine(fbfd, 0, 100, 300, makepixel(0, 0, 255));
            DrawLine(fbfd, 0, 100, 400, makepixel(0, 0, 255));
            DrawLine(fbfd, 200, 300, 400, makepixel(0, 0, 255));
            DrawLine(fbfd, 400, 500, 400, makepixel(0, 0, 255));
            DrawLine(fbfd, 600, 700, 400, makepixel(0, 0, 255));

            close(fbfd);

            return 0;
    }
    ```
  - VNC에서는 보이지않지만, LCD 화면으로는 보인다!!!
  - 예제돌리기
    - point.c : 점찍기
    - line.c  : 라인그리기 DrawLine
    - circle.c :  마젠타색으로 원 그리기
    - face.c : 화면칠함, 느리다, 일일이 한바이트씩 쓰고있어서 느린거고 성능이 생명이다. 이런거 진짜 고민많이해야되는 멀티미디어 성능 끌어올리는게 관건임
  - write: 커널함수, 시스템콜
  - fwrite : C표준
  - 커널에서는 write : 펌웨어와 범용OS 
  - 어플단에서 커널함수 호출 다이렉트 절대금지, 그래서 인터럽트 -> 인터럽트 핸들러에서 특정함수 호출, 커널의 다른이름으로 함수 호출할 수 있다.
  - 범용의 드라이버 구조를 배우고싶으면 커널강의 들어야함... -> 오버헤드가 크다! 
    - 한번에 4바이트씩 가지말고, 전체를 잡아가지고 한번만 부르자 버퍼링 기법..!!!
    - APP과 커널은 서로 다른공간.., 커널의 메모리매니지먼트단에서 함수를 만들어줘요, 함수를 만들어서 
    - 드라이버는 그함수로, 다이렉트로 가면 안됨 
      - 카피 프롬 유저 : 
      - 카피 투 유저 : 
    - 오버헤드가 장난이 아니야. OS 시큐리티 떄문에.. 성능이 떨어져,, 대량의 데이터를 와따리가따리, 범용시스템에서 트릭
      - 이런 트릭이 바로 nmap : 메모리 맵핑  
  - 새로운 예제 : mmap 기능! 포인터하나가 넘어가면 매핑!!! 
    - face_mmap.c : 직접 커널영역에다가 써버리는거야, 그럼 카피프롬, write함수에다가 안써도 됨...
      - 괭장히 편해지는거야

    - fbdraw.c  
  - 이거 다 할수 없어서 프레임워크가 존재하는거야

# day2 3교시
> BMP
  - 여기서 할꺼임
  ```
  pi@raspberrypi:~/iot-mm/ch02/03.bmpViewer $ ls
  bmpHeader.c  bmpHeader.h  bmpViewer.c
  ```
  - 컴파일방법 : gcc *.c
  -
```
pi@raspberrypi:~/iot-mm/ch02/03.bmpViewer $ cat bmpHeader.h
#ifndef __BMP_FILE_H__
#define __BMP_FILE_H__

typedef struct __attribute__((__packed__)) {
    unsigned short bfType;          /* BM 표시 : "BM" (2 글자) 문자 */
    unsigned int bfSize;            /* 파일의 크기 : 4 바이트 정수  */
    unsigned short bfReserved1;     /* 추후의 확장을 위해서 필드(reserved) */
    unsigned short bfReserved2;     /* 추후의 확장을 위해서 필드(reserved) */
    unsigned int bfOffBits;         /* 실제 이미지까지의 오프셋(offset) : 바이트 */
} BITMAPFILEHEADER;

typedef struct {
    unsigned int biSize;            /* 현 구조체의 크기 : 4바이트 */
    unsigned int biWidth;           /* 이미지의 폭(픽셀단위) : 4바이트 */
    unsigned int biHeight;          /* 이미지의 높이(픽셀단위) : 4바이트 */
    unsigned short biPlanes;        /* 비트 플레인 수 (항상1) : 2바이트 */
    unsigned short biBitCount;      /* 픽셀 당 비트 수 : 2바이트 */
    unsigned int biCompression;     /* 압축 유형 : 4바이트 */
    unsigned int SizeImage;         /* 이미지의 크기(압축 전의 바이트 단위) : 4바이트 */
    unsigned int biXPelsPerMeter;   /* 가로 해상도  : 4바이트 */
    unsigned int biYPelsPerMeter;   /* 세로 해상도 : 4바이트 */
    unsigned int biClrUsed;         /* 실제 사용되는 색상수 : 4바이트 */
    unsigned int biClrImportant;    /* 중요한 색상 인덱스(0 인 경우 전체) : 4바이트 */
} BITMAPINFOHEADER;

typedef struct {
    unsigned char rgbBlue;
    unsigned char rgbGreen;
    unsigned char rgbRed;
    unsigned char rgbReserved;
} RGBQUAD;

#endif /* __BMP_FILE_H__ */
  
```
  - 
```cpp
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <unistd.h>
#include <fcntl.h>
#include <limits.h>                     /* USHRT_MAX 상수를 위해서 사용 */
#include <sys/ioctl.h>
#include <sys/types.h>
#include <sys/mman.h>
#include <linux/fb.h>

#include "bmpHeader.h"

#define FBDEVFILE            "/dev/fb0"
#define NO_OF_COLOR    24/8             // 24비트(true color) BMP 이미지

#define LIMIT_UINT(n) (n>UINT_MAX)?UINT_MAX:(n<0)?0:n
#define LIMIT_USHRT(n) (n>USHRT_MAX)?USHRT_MAX:(n<0)?0:n
#define LIMIT_UBYTE(n) (n>UCHAR_MAX)?UCHAR_MAX:(n<0)?0:n

extern int readBmp(char *filename, unsigned char **pData, int *cols, int *rows);

#ifdef BPP16
unsigned short makepixel(unsigned char r, unsigned char g, unsigned char b)
{
        return (unsigned short)( ((r>>3)<<11) | ((g>>2)<<5) | (b>>3) );
}
#else
unsigned int makepixel(unsigned char r, unsigned char g, unsigned char b)
{
        return (unsigned int)( (r<<16) | (g<<8) | (b) );
}
#endif

int main(int argc, char **argv)
{
    int cols, rows;
    int width, height;
    unsigned char *pData;
    unsigned char r, g, b;
#ifdef BPP16
    unsigned short *pBmpData, *pfbmap, pixel;
#else
        unsigned int *pBmpData, *pfbmap, pixel;
#endif
    struct fb_var_screeninfo vinfo;
    int fbfd;
    int x, y, k, t;

    if(argc != 2) {
        printf("\nUsage: ./%s xxx.bmp\n", argv[0]);
        return 0;
    }

    /* 프레임버퍼를 오픈한다. */
    // 프레임버퍼 디바이스 파일
    fbfd = open(FBDEVFILE, O_RDWR);
    if(fbfd < 0) {
        perror("open( )");
        return -1;
    }
    // 디스크립터
    /* 현재 프레임버퍼에 대한 고정된 화면 정보를 얻어온다. */
    if(ioctl(fbfd, FBIOGET_VSCREENINFO, &vinfo) < 0) {
        perror("ioctl( ) : FBIOGET_VSCREENINFO");
        return -1;
    }

    cols = vinfo.xres;
    rows = vinfo.yres;

    /* BMP 출력을 위한 변수의 메모리 할당 */
        pData = (unsigned char *)malloc(cols * rows * sizeof(unsigned char) * NO_OF_COLOR);

    /* 프레임버퍼에 대한 메모리 맵을 수행한다. */
#ifdef BPP16
        pfbmap = (unsigned short *)mmap(0, cols*rows*2,
#else
    pfbmap = (unsigned int *)mmap(0, cols*rows*4,
#endif
                PROT_READ|PROT_WRITE,  MAP_SHARED, fbfd, 0);
    if((unsigned)pfbmap == (unsigned)-1) {
        perror("mmap( )");
        return -1;
    }

    /* BMP 파일에서 헤더 정보를 가져온다. */
    if(readBmp(argv[1], &pData, &width, &height) < 0) {
        perror("readBMP( )");
        return -1;
    }

    printf("\nWidth : %d, Height : %d\n", width, height);

    /* 24 비트의 BMP 이미지 데이터를 16or32비트 이미지 데이터로 변경 */
    for(y = 0; y < height; y++) {
        k = (height-y-1)*width*NO_OF_COLOR;
        t = y*cols;

        for(x = 0; x < width; x++) {
            b = LIMIT_UBYTE(pData[k+x*NO_OF_COLOR+0]);
            g = LIMIT_UBYTE(pData[k+x*NO_OF_COLOR+1]);
            r = LIMIT_UBYTE(pData[k+x*NO_OF_COLOR+2]);
#ifdef BPP16
            pixel = LIMIT_USHRT(makepixel(r, g, b));
#else
                        pixel = LIMIT_UINT(makepixel(r, g, b));
#endif
            pfbmap[t+x] = pixel;
        };
    };

#ifdef BPP16
    munmap(pfbmap, cols*rows*2);
#else
        munmap(pfbmap, cols*rows*4);
#endif

    free(pData);

    close(fbfd);
    return 0;
}

```
  - inline 함수는 옵티마이져 레벨 1부터 먹는다
  -  gcc -O1 v4l2_fb.c
    - 옵티마이져 레벨을 1로 해야 인라인 키워드가 먹는다.
    - 롬 사이즈 부족할때, KEA 강의 @@@@@들어라!!!!
  - RGB to YUV 최대한 빨리 코딩하는 공식 
```cpp
pi@raspberrypi:~/iot-mm/ch03/01.camera $ cat v4l2_fb.c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <fcntl.h>        /* Low level I/O를 위해서 사용 */
#include <errno.h>
#include <sys/types.h>
#include <sys/mman.h>
#include <sys/ioctl.h>
#include <asm/types.h>       /* <videodev2.h> 파일을 위해서 사용 */
#include <linux/videodev2.h> /* Video4Linux2를 위한 헤더파일 */
#include <linux/fb.h>
#include <unistd.h>

#define MEMZERO(x) memset(&(x), 0, sizeof(x))    /* 변수 초기화를 위한 매크로 */

#define VIDEODEV        "/dev/video0"    /* Pi Camera를 위한 디바이스 파일 */
#define FBDEV           "/dev/fb0"       /* 프레이버퍼를 위한 디바이스 파일 */
#define WIDTH           800              /* 캡쳐받을 영상의 크기 */
#define HEIGHT          480

/* Video4Linux에서 사용할 영상 저장을 위한 버퍼 */
struct buffer {
    void * start;
    size_t length;
};

static int fd = -1;              /* Pi Camera의 장치의 파일 디스크립터 */
struct buffer *buffers = NULL;   /* Pi Camera의MMAP를 위한 변수 */
static int fbfd = -1;            /* 프레임버퍼의 파일 디스크립터 */
#ifdef BPP16
static short *fbp = NULL;        /* 프레임버퍼의 MMAP를 위한 변수 */
#else
static int *fbp = NULL;        /* 프레임버퍼의 MMAP를 위한 변수 */
#endif
static struct fb_var_screeninfo vinfo;  /* 프레임버퍼의 정보 저장을 위한 구조체 */

static void processImage(const void *p);
static int readFrame();
static void initRead(unsigned int buffer_size);

static void initDevice()
{
    struct v4l2_capability cap;      /* 비디오 장치에 대한 기능을 조사한다. */
    struct v4l2_format fmt;
    unsigned int min;

    if(ioctl(fd, VIDIOC_QUERYCAP, &cap) < 0) { /* 장치가 V4L2를 지원하는지 조사 */
        if(errno == EINVAL) {// 에러넘버 어쩌고 디바이스 없음
            perror("/dev/video0 is no V4L2 device");
            exit(EXIT_FAILURE);
        } else {// 상세정보 출력 가능...
            perror("VIDIOC_QUERYCAP");
            exit(EXIT_FAILURE);
        }
    }

    // 정상이라면 여기에 도달
    /* 장치가 영상 캡쳐 기능이 있는지 조사 */
    if(!(cap.capabilities & V4L2_CAP_VIDEO_CAPTURE)) {
        perror("/dev/video0 is no video capture device");
        exit(EXIT_FAILURE);
    }

    MEMZERO(fmt);
    fmt.type = V4L2_BUF_TYPE_VIDEO_CAPTURE;
    fmt.fmt.pix.width = WIDTH;
    fmt.fmt.pix.height = HEIGHT;
    fmt.fmt.pix.pixelformat = V4L2_PIX_FMT_YUYV;
    fmt.fmt.pix.field = V4L2_FIELD_NONE;

    
    // 셋포멧, 겟포맷
    //ioctl 공부해라
    if(ioctl(fd, VIDIOC_S_FMT, &fmt) < 0) {
        perror("VIDIOC_S_FMT");
                close(fd);
        exit(EXIT_FAILURE);
    }

        if(ioctl(fd, VIDIOC_G_FMT, &fmt) < 0) {
                perror("VIDIOC_G_FMT\n");
                close(fd);
                exit(EXIT_FAILURE);
        }

    if (fmt.fmt.pix.pixelformat != V4L2_PIX_FMT_YUYV) {
        perror("Requested pixel format not supported\n");
        exit(EXIT_FAILURE);
    }

    /* 영상의 최소 크기를 구함 */
#ifdef DEBUG
        printf("-->fmt.fmt.pix.bytesperline:%d\n", fmt.fmt.pix.bytesperline); //1600
        printf("-->fmt.fmt.pix.width:%d\n", fmt.fmt.pix.width); //800
        printf("-->fmt.fmt.pix.height:%d\n", fmt.fmt.pix.height); //480
        printf("-->fmt.fmt.pix.sizeimage:%d\n", fmt.fmt.pix.sizeimage);
#endif

    min = fmt.fmt.pix.width * 2;//픽셀당 두바이트.. 폭을 읽었는데, 맞는지 어쩐지 검사를 한다.
    if(fmt.fmt.pix.bytesperline < min)
        fmt.fmt.pix.bytesperline = min;
    min = fmt.fmt.pix.bytesperline * fmt.fmt.pix.height;
    if(fmt.fmt.pix.sizeimage < min)
        fmt.fmt.pix.sizeimage = min;

#ifdef DEBUG
        printf("-->fmt.fmt.pix.bytesperline:%d\n", fmt.fmt.pix.bytesperline);
        printf("-->fmt.fmt.pix.sizeimage:%d\n", fmt.fmt.pix.sizeimage);
#endif

    /* 영상 읽기를 위한 초기화 */
    initRead(fmt.fmt.pix.sizeimage);
}

// 그 크기만큼 읽어라
static void initRead(unsigned int buffer_size)
{
    /* 메모리를 할당한다 */
    buffers = (struct buffer*)calloc(1, sizeof(*buffers));// 멜록플러스 클리어
    if(!buffers) {
        perror("Out of memory");
        exit(EXIT_FAILURE);
    }

    /* 버퍼를 초기화 한다. */
    buffers[0].length = buffer_size;
    buffers[0].start = malloc(buffer_size);
    if(!buffers[0].start) {
        perror("Out of memory");
        exit(EXIT_FAILURE);
    }
}




//위에서 준비해 온 메모리... 

#define NO_OF_LOOP    3000

static void mainloop()
{
    unsigned int count = NO_OF_LOOP;
    while(count-- > 0) {      /* 100회 반복 */
        for(;;) {
            fd_set fds;
            struct timeval tv;
            int r;

            /* fd_set 구조체를 초기화하고 비디오 장치를 파일 디스크립터를 설정한다. */
            FD_ZERO(&fds);
            FD_SET(fd, &fds);

            /* 타임아웃(Timeout)을 2초로 설정한다. */
            tv.tv_sec = 2;
            tv.tv_usec = 0;

                        printf("Waiting Image...\n");
            r = select(fd + 1, &fds, NULL, NULL, &tv);  /* 비디오 데이터가 올때까지 대기 */
            // 셀렉트 함수.. !!!!!!!!!!!!!!!!!!
            // 셀렉트는 IO 멀티플랙싱 함수.. 비디오캡쳐링 하는데 카메라 두개,, 믹스해가지고 여기다가
            switch(r) {
                case -1:                         /* select( ) 함수 에러시의 처리 */
                      if(errno != EINTR) {
                          perror("select( )");
                          exit(EXIT_FAILURE);
                      }
                      break;
                case 0:                           /* 타임아웃시의 처리 */
                      perror("select timeout");
                      exit(EXIT_FAILURE);
                      break;
            };

                        printf("Capturing Image~~\n");
            if(readFrame()) break;                /* 현재의 프레임을 읽어서 표시 */
        };
    };
}

static int readFrame( )
{
  //카메라에서 하나의 프레임을 읽어온다.
  if(read(fd, buffers[0].start, buffers[0].length) < 0) {
        perror("read( )");
        exit(EXIT_FAILURE);
  }

  // 읽어온 프레임을 색상 공간 등을 변경하고 화면에 출력한다.
  processImage(buffers[0].start);

  return 1;
}

/* unsigned char의 범위를 넘어가지 않도록 경계 검사를 수행다. */
inline int clip(int value, int min, int max)
{
    return(value > max ? max : value < min ? min : value);
}

/* YUYV를 RGB16or32으로 변환한다. */
static void processImage(const void *p)
{
    unsigned char* in =(unsigned char*)p;
    int width = WIDTH;
    int height = HEIGHT;

    //unsigned short pixel;
        unsigned int pixel;
    int istride = WIDTH*2;  /* 이미지의 폭을 넘어가면 다음 라인으로 내려가도록 설정 */
    int x, y, j;
    int y0, u, y1, v, r, g, b;
    long location = 0;
    for(y = 0; y < height; ++y) {
        for(j = 0, x = 0; j < vinfo.xres * 2; j += 4, x += 2) {
            if(j >= width*2) {  /* 현재의 화면에서 이미지를 넘어서는 빈 공간을 처리 */
                 location++; location++;
                 continue;
            }
            /* YUYV 성분을 분리 */
            y0 = in[j] - 16 ;
            u = in[j + 1] - 128;
            y1 = in[j + 2] - 16;
            v = in[j + 3] - 128;

            /* YUV를 RGB로 전환 */
            r = clip((298 * y0 + 409 * v + 128) >> 8, 0, 255);
            g = clip((298 * y0 - 100 * u - 208 * v + 128) >> 8, 0, 255);
            b = clip((298 * y0 + 516 * u + 128) >> 8, 0, 255);
#ifdef BPP16
            pixel =((r>>3)<<11)|((g>>2)<<5)|(b>>3);  /* 16비트 컬러로 전환 */
#else
                        pixel = ( (r<<16) | (g<<8) | (b) );  /* 32비트 컬러로 전환 */
#endif
            fbp[location++] = pixel;

            /* YUV를 RGB로 전환 */
            r = clip((298 * y1 + 409 * v + 128) >> 8, 0, 255);
            g = clip((298 * y1 - 100 * u - 208 * v + 128) >> 8, 0, 255);
            b = clip((298 * y1 + 516 * u + 128) >> 8, 0, 255);
#ifdef BPP16
            pixel =((r>>3)<<11)|((g>>2)<<5)|(b>>3);  /* 16비트 컬러로 전환 */
#else
                        pixel = ( (r<<16) | (g<<8) | (b) );  /* 32비트 컬러로 전환 */
#endif
            fbp[location++] = pixel;
        };
        in += istride;
    };
}

static void uninitDevice()
{
#ifdef BPP16
    long screensize = vinfo.xres * vinfo.yres * 2;
#else
        long screensize = vinfo.xres * vinfo.yres * 4;
#endif

    free(buffers[0].start);
    free(buffers);
    munmap(fbp, screensize);
}


// 메인에 갔더니
int main(int argc, char ** argv)
{
    long screensize = 0;

    /* 장치 열기 */
    /* Pi Camera 열기 */
    fd = open(VIDEODEV, O_RDWR| O_NONBLOCK, 0);
    if(fd == -1) {
        perror("open( ) : video device");
        return EXIT_FAILURE;
    }

    /* 프레임버퍼 열기 */
    fbfd = open(FBDEV, O_RDWR);// 프레임버퍼 디스크립터
    if(fbfd == -1) {
        perror("open( ) : framebuffer device");
        return EXIT_FAILURE;
    }

    /* 프레임버퍼의 정보 가져오기 */
    if(ioctl(fbfd, FBIOGET_VSCREENINFO, &vinfo) == -1) {
         perror("Error reading variable information.");
         exit(EXIT_FAILURE);
    }

    /* mmap( ) : 프레임버퍼를 위한 메모리 공간 확보 */
#ifdef BPP16
    screensize = vinfo.xres * vinfo.yres * 2;
    fbp = (short *)mmap(NULL, screensize, PROT_READ | PROT_WRITE, MAP_SHARED, fbfd, 0);
#else
    // 정보의 스크린사이즈만큼.....
    screensize = vinfo.xres * vinfo.yres * 4;
    fbp = (int *)mmap(NULL, screensize, PROT_READ | PROT_WRITE, MAP_SHARED, fbfd, 0);
    // 포인터에다가 저장...

#endif
        if((int)fbp == -1) {
        perror("mmap() : framebuffer device to memory");
        return EXIT_FAILURE;
    }
    memset(fbp, 0, screensize);
    // 처음에는 0으로 초기화(스크린 사이즈만큼)


    // 여기가 제일 중요!@!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
    initDevice();            /* 장치 초기화 */
    mainloop();              /* 캡쳐 실행 */
    uninitDevice();          /* 장치 해제 */

    /* 장치 닫기 */
    close(fbfd);
    close(fd);

    return EXIT_SUCCESS;     /* 애플리케이션 종료 */
}


```
  - IO 멀티플렉싱 함수 select, poll, epoll @@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@ 이거 해야된다 
    ```
    r = select(fd + 1, &fds, NULL, NULL, &tv);  /* 비디오 데이터가 올때까지 대기 */
    // 셀렉트 함수.. !!!!!!!!!!!!!!!!!!
    // 셀렉트는 IO 멀티플랙싱 함수.. 비디오캡쳐링 하는데 카메라 두개,, 믹스해가지고 여기다가
    ```
  - 결국 카메라이미지를 디스플레이에 바로 떄려박는 예제
  - 밥먹고 멀티미디어 프레임워크.. 
# day2 4교시 (점심이후)
> 파이카메라
  - pi@raspberrypi:~/iot-mm/ch03/02.camera_py $ cat 01.camera_py2.py
    ```py
    #coding: utf-8

    import picamera
    import time
    with picamera.PiCamera() as camera:
            res = int(raw_input('Resolution(1:320x240, 2:640x480, 3:1024x768)? '))
            #파이썬 버전2
            if res == 3:
                    camera.resolution = (1024, 768)
            elif res == 2:
                    camera.resolution = (640, 480)
            else:
                    camera.resolution = (320, 240)

            filename = raw_input('File Name? ')
            camera.start_preview()
            time.sleep(1)
            camera.stop_preview()
            camera.capture(filename + '.jpg')
    ```
  - raw_input() 을 input으로 변경하면 파이썬3에서도 동작함..
  - 대충.. 실습해봄
    ```
    pi@raspberrypi:~/iot-mm/ch03/03.video $ gcc -O1 bmpCapture.c
    pi@raspberrypi:~/iot-mm/ch03/03.video $ ./a.out 
    Waiting Image...
    Capturing Image~~
    pi@raspberrypi:~/iot-mm/ch03/03.video $ ls
    a.out  bmpCapture.c  bmpHeader.h  capture.bmp
    ```
  - bmpCapture.c 설명  : 어쩌고저쩌고.. 그냥 코드읽어주기...  
  - 시간투자 하기싫다. 나도 여기다가 시간투자 안했다.(인정하네.....)
  - 그냥 도는 코드 대충 긁어다가 가져다가 강의하는거야...
  - 메뉴얼도 구글 검색하면 대충 나올꺼에요
  - 동영상이 레코딩이 된다...........
  - https://picamera.readthedocs.io/en/release-1.10/api_camera.html
  - 위에 파이카메라 독스 보고 열심히 따라배워라...
  - 
    ```py3
    #coding: utf-8

    import picamera
    from tkinter import *

    def rec_vdo():
        if res.get() == 3:
            camera.resolution = (1024, 768)
        elif res.get() == 2:
            camera.resolution = (640, 480)
        else:
            camera.resolution = (320, 240)
            
        if rot.get() == 4:
            camera.rotation = int(rotation[3][0])
        elif rot.get() == 3:
            camera.rotation = int(rotation[2][0])
        elif rot.get() == 2:
            camera.rotation = int(rotation[1][0])
        else:
            camera.rotation = int(rotation[0][0])
            
        filename = et.get()
        camera.start_recording(filename + '.h264')
        camera.wait_recording(5)
        camera.stop_recording()
        
        label4 = Label(root, text='Movie Saved')
        label4.pack()
        
    root = Tk()

    with picamera.PiCamera() as camera:
        resolution = [('320x240', 1), ('640x480', 2), ('1024x768', 3)]
        rotation = [('0', 1), ('90', 2), ('180', 3), ('270', 4)]
        
        res = IntVar()
        res.set(1);
        
        rot = IntVar()
        rot.set(1)
        
        label1 = Label(root, text='Resolution')
        label1.pack()	
        for text, mode in resolution:
            rb1 = Radiobutton(root, text=text, 
                    variable=res, value=mode)
            rb1.pack(anchor='w')
            
        label2 = Label(root, text='Rotation')
        label2.pack()	
        for text, mode in rotation:
            rb2 = Radiobutton(root, text=text, 
                    variable=rot, value=mode)
            rb2.pack(anchor='w')

        label3 = Label(root, text='Save Filename')
        label3.pack()
        et = Entry(root)
        et.pack()
        
        btn = Button(root, text='Picture', width=10, command=rec_vdo)
        btn.pack()
        
        root.mainloop()
    ```
  -   
# day2 5교시
>
  - 미디어 프레임워크 정리 : 3장앞단, 2장마지막쪽 : 종이책 71, 피피티132
    - from 미디어텍, 스마트폰은 멀티미디어가 생명
      - 안드로이드 순수앱 : Java API로 만들어짐, 우리는 C레벨, 하드웨어 컨트롤은 C레벨 맞음 어디를가도
      - 자바 API가 준비되어 있다!! : 이게 바로 안드로이드 어플리케이션 프레임워크(자바코드)
      - 프레임워크가 붙으면 생각을 해야하는게, 라이브러리라고만 명시가 되면 호출해다 알아서 쓰면 되는데, 
      - 프레임워크라면 라이브러리랑 다르다
        - 프레임워크라면, 자바들이 구현이 되어있지만, 어떤건 New를 내가 쓸수 있지만 어떤것들은 프레임워크만 New를 쓸수있거나, 호출하는 시점을 내가 정할 수 없을수 있다. 지스트리머 코딩한번 X
      - 멀티미디어는 매우매우매우 복잡하다. 내가 할수 없다, 그냥 쉽게할수 있는 분야가 아니다. 단순하게 자바의 문법만 안다고 만드는게 아니다. 안드로이드 프레임워크를 이해해야지만 만들 수 있다. 단순하게 불러서 쓰는 라이브러리의 수준이 아니다.
      - 자바에 카메라 라는 클래스가 있다면 진짜 디바이스를 제어하는코드가 없다.
      - 자바는 인터프리팅 + 컴파일 : 자바VM 자바는 VM을 쓰셔야되요, 그래서 안드로이드에 vm이 탑재됨.
      - vm이 가비지컬랙션도 하고 다 해줘..
      - 시스템콜은 c/c++ 뿐이다.. 
      - 안드로이드 프레임워크는 자바이지만,
      - 안드로이드 네이티브 프레임워크는 c/c++ , 네이티브 카메라에
      - 어플의 입장에서는 커널의 api를 직접 부를수는 없는거에여, 
      - 프레임워크를 만드는 사람은 다 알아야하지만,+동기화처리(스레드 세이프), 최종어플을 만드는 사람은 몰라도 되
      - 그래서 프레임워크를 쓴다.
      - 안드로이드는 미디어 프레임워크를 자체적으로 다 만들어져있다.
      - 실제 미디어프레임워크는 어마어마하게 복잡하다.. 서비스가 매우 개 핵복잡
      - 오디오프링거, 그래픽:서피스플링거, 미디어:미디어프레임워크
      - 안드로이드도 OpenMAX기반으로 만들어져있어. ->> 구글링을 해보면 크로노스그룹에서 미디어프레임 표준으로 만든거야
      - 3단계 : 디바이스 레이어 / 중간(미들웨어) / APP 레이어
      - 코덱이나 이런것에 대한 레이어이다.. 분석해보면.. 
        - 디바이스단도 이 룰을 지켜라.. 룰을 지켜서 코딩이 되어있어, 오픈맥스를 지켜서 자체적인 자바인터페이스
  - sudo apt-get install motion
  - 명령어를 내리면 디폴트 실행이지만, 모션은 디폴트가 실행 안되는 모드
  - 왜 디폴트 실행안움직이니까, 모션 움직이면 레코딩을 하니.. 디폴트를 막음
  - 모션이라는 서비스 돌려보기 위해서 뭐 하는중...
  - 외부에서 접근 가능하게 하는 부분
    - stream_localhost off ->> 
    - 위치 : root@raspberrypi:/etc/motion# vim motion.conf
    - stickybit 설정하면 체인지own 안하고 쓸수있지않음?
    ```
    pi@raspberrypi:~ $ ps -ef | grep motion
    motion     413     1  6 16:27 ?        00:00:02 /usr/bin/motion
    ```
  - 파일 만들어지는거 확인해봐야됭
    - 의미있는 기능추가나 오픈소스라면 motion 을 찾아서 해야되는거지..
    - 반드시 알아야하는 오픈소스를 써먹기 위한 것들..!!
    - 쉘스크립트 주기적으로 빽업 혹은 삭제 쉘스크립트도 배우는게 좋다->> 위험하다. OFF

```py
pi@raspberrypi:~/iot-mm/ch04/01.pi_cctv $ cat pi_cctv.py
# Web streaming example
# Source code from the official PiCamera package
# http://picamera.readthedocs.io/en/latest/recipes2.html#web-streaming


#사용된 모듈들
import io#
import picamera
import logging
import socketserver
from threading import Condition#내부에서 쓰레드, 병렬처리 하는중
from http import server# 당연히 http서버이니..

PAGE="""\
<html>
<head>
<title>Raspberry Pi - Surveillance Camera</title>
</head>
<body>
<center><h1>Raspberry Pi - Surveillance Camera</h1></center>
<center><img src="stream.mjpg" width="640" height="480"></center>
</body>
</html>
"""

class StreamingOutput(object):
    def __init__(self):
        self.frame = None
        self.buffer = io.BytesIO()
        self.condition = Condition()# 쓰레드 컨디션 체크 하는 객체 하나 만들어짐

    def write(self, buf):
        if buf.startswith(b'\xff\xd8'):
            # New frame, copy the existing buffer's content and notify all
            # clients it's available
            self.buffer.truncate()
            with self.condition:
                self.frame = self.buffer.getvalue()
                self.condition.notify_all()
            self.buffer.seek(0)
        return self.buffer.write(buf)

class StreamingHandler(server.BaseHTTPRequestHandler):
    def do_GET(self):
        if self.path == '/':
            self.send_response(301)
            self.send_header('Location', '/index.html')
            self.end_headers()
        elif self.path == '/index.html':
            content = PAGE.encode('utf-8')
            self.send_response(200)
            self.send_header('Content-Type', 'text/html')
            self.send_header('Content-Length', len(content))
            self.end_headers()
            self.wfile.write(content)
        elif self.path == '/stream.mjpg':
            self.send_response(200)
            self.send_header('Age', 0)
            self.send_header('Cache-Control', 'no-cache, private')
            self.send_header('Pragma', 'no-cache')
            self.send_header('Content-Type', 'multipart/x-mixed-replace; boundary=FRAME')
            self.end_headers()
            try:
                while True:
                    with output.condition:
                        output.condition.wait()
                        frame = output.frame
                    self.wfile.write(b'--FRAME\r\n')
                    self.send_header('Content-Type', 'image/jpeg')
                    self.send_header('Content-Length', len(frame))
                    self.end_headers()
                    self.wfile.write(frame)
                    self.wfile.write(b'\r\n')
            except Exception as e:
                logging.warning(
                    'Removed streaming client %s: %s',
                    self.client_address, str(e))
        else:
            self.send_error(404)
            self.end_headers()

class StreamingServer(socketserver.ThreadingMixIn, server.HTTPServer):
    allow_reuse_address = True
    daemon_threads = True # 데몬화 처리 필요!!

with picamera.PiCamera(resolution='640x480', framerate=24) as camera:
    output = StreamingOutput()
    #Uncomment the next line to change your Pi's Camera rotation (in degrees)
    #camera.rotation = 90
    camera.start_recording(output, format='mjpeg')
    # 캡쳐한걸 계속 내보내는중임
    try:
        address = ('', 8000)
        server = StreamingServer(address, StreamingHandler)
        server.serve_forever()
    finally:
        camera.stop_recording()p
```
  - daemon_threads = True # 데몬화 처리 필요!! 
  - 왜 대몬을 써야하는가??!?!?!?!?
  - http://edu.kosta.or.kr/
# day2 6교시
>
  - cmake는 make를 자동으로 만들어주는 툴
  - 원래 make를 빌드를 자동으로 해주는 시스템 -> 메이크파일을 작성하는걸 배울껀데요 기본으로 GCC 기반으로 배울꺼지
  - 메이크로 라이브러리 빌드, 실행파일 빌드... 그런다음에 메이크 해가지고 내마음대로 빌드할 수 있어요
  - 메이크파일의 룰이 너무 빡세고 어려워.. + 유닉스다르고 리눅스 다르고 우분투다르고 어쩌고.. 
  - 똑같은 cmake 파일을 만들면 어디서나 크로스플랫폼 make이다 그게 바로 Make
  - 공부해아할것 : 리눅스 커맨드, 쉘스크립트 , 메이크, 빌드시스템
# day2 7교시
> 오픈소스 mjpg 스트리밍 코드(오픈소스)
  ```
  sudo apt-get install git
  git clone https://github.com/jacksonliam/mjpg-streamer
  sudo apt-get install cmake python-imaging libjpeg-dev build-essential
  cd mjpg-streamer/
  cd mjpg-streamer-experimental/
  make CMAKE_BUILD_TYPE=Release
  make
  cat mjpg_streamer.c 
  ./mjpg_streamer -i "./input_uvc.so -y" -o "./output_http.so -w ./www" 
  
  ```
  - 이제 소스코드는 넘쳐나는데, 리빌드를 하고, 라이브러리에서 리빌드하고 땡겨 쓰는법을 배워야함
  - 그래서 빌드시스템과 라이브러리를 쓰는법을 배워야함
  - 내가 원하는쪽에다가 다른데다가 배치하고 자동으로 다 배치할 수 있어요,... ->> 빌드시스템에서 배울수 있어요..
  - 
# 질문사항
  - 지스트리머, 오픈맥스, SDL 차이점이 궁금합니다
  - Fragments