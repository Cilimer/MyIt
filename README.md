import java.io.File;
import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.FileOutputStream;
import java.io.IOException;
import java.nio.channels.FileChannel;

public class FileCopy {
	/**
	 * 背景:从网上下了个资料,加压之后,发现有很多子目录,就想将各个子目录下的文件复制到一个文件夹中,因为手动太麻烦,考虑用Java写个小程序来跑一下.
	 * 功能:将E盘下 from文件中, 各个子目录下的 以AAA开头的文件,复制到F盘下的to文件夹下.
	 * 如果有多层目录,可以考虑加上递归操作.这里就不加了,以后有需要再扩展
	 */
	public static void main(String[] args) throws FileNotFoundException, IOException, ClassNotFoundException {
		//源文件夹
		String path = "E:\\from";
		//目标文件夹
		String fpath = "F:\\to";
		File file = new File(path);
		
		//判断源文件夹是个目录
		if(file.isDirectory()){
			File[] files = file.listFiles();
			//遍历目录下的子目录
			for (int i = 0; i < files.length; i++) {
				File f = files[i];
				//判断子文件是个目录
				if(f.isDirectory()){
					//获取子目录的所有子文件
					File[] f2 = f.listFiles();
					for (int j = 0; j < f2.length; j++) {
						File ff = f2[j];
						String fname = ff.getName();
						if(fname.startsWith("AAA")){
							//执行copy操作
							fileChannelCopy(ff,new File(fpath + File.separator + fname));
						}
					}
				}
			}
		}
	}
	
	//执行复制操作
	public static void fileChannelCopy(File s, File t) {
        FileInputStream fi = null;
        FileOutputStream fo = null;
        FileChannel in = null;
        FileChannel out = null;
        try {
            fi = new FileInputStream(s);
            fo = new FileOutputStream(t);
            in = fi.getChannel();//得到对应的文件通道
            out = fo.getChannel();//得到对应的文件通道
            in.transferTo(0, in.size(), out);//连接两个通道，并且从in通道读取，然后写入out通道
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            try {
                fi.close();
                in.close();
                fo.close();
                out.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }
}
