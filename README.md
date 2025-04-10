<?php
class cnmoi 
{
    private function connect()
    {
        $con=mysql_connect("localhost","httt","123456");
        if(!$con)
        {
            echo 'loi ket noi';
            exit();
        }
        else
        {
            mysql_select_db("middle");
            mysql_query("SET NAMES UTF-8");
            return $con;
        }
    }
    public function xuatkhachhang($sql)
    {
        $link = $this ->connect();
        $ketqua = mysql_query($sql, $link);
        $i = mysql_num_rows($ketqua);
        if($i>0)
        {
            $dulieu=array();
            while($row=mysql_fetch_array($ketqua))
            {
                $makh=$row['makh'];
                $hoten=$row['hoten'];
                $diachi=$row['diachi'];
                $sdt=$row['sdt'];
                $madh=$row['madh'];
                $ngaydat=$row['ngaydat'];
                $tongtien=$row['tongtien'];
                $dulieu[]=array('makh'=>$makh,'hoten'=>$hoten, 'diachi'=>$diachi,'sdt'=>$sdt, 'madh'=>$madh, 'ngaydat'=>$ngaydat, 'tongtien'=>$tongtien);
            }
            header("connet-Type:application/json; charset=utf-8");
            echo json_encode($dulieu);
        }
    }
}
?>
-------
<?php
include("../class/clscnmoi.php");
$p = new cnmoi();

$p->xuatkhachhang("
    SELECT kh.makh, kh.hoten, kh.diachi, kh.sdt, dh.madh, dh.ngaydat, dh.tongtien
    FROM khachhang kh
    JOIN donhang dh ON kh.makh = dh.makh
");
?>
------

<?php
class myapi
{
	private function docjson($url)
	{
		$client=curl_init($url);
		curl_setopt($client,CURLOPT_RETURNTRANSFER,1);
		$response=curl_exec($client);
		$results=json_decode($response);
		return $results;
	}
	public function xuatdssv($url)
	{
		$results=$this->docjson($url);
		echo '<table width="888" border="1" align="center" cellpadding="0" cellspacing="0">
				<tbody>
				  <tr></tr>
				</tbody>
				<tbody>
				  <tr>
                    <td  align="center" valign="middle">STT</td>
					<td  align="center" valign="middle">MAKH</td>
					<td  align="center" valign="middle">HỌ TÊN</td>
					<td  align="center" valign="middle">ĐỊA CHỈ</td>
					<td  align="center" valign="middle">SDT</td>
					<td  align="center" valign="middle">MÃ ĐƠN HÀNG</td>
                    <td  align="center" valign="middle">NGÀY ĐẶT</td>
                    <td  align="center" valign="middle">TỔNG TIỀN</td>
                   
				  </tr>';	
		$dem=1;
		foreach($results as $data)
		{
		echo '<tr>
			<td align="center" valign="middle">'.$dem.'</td>
			<td align="center" valign="middle">'.$data->makh.'</td>
			<td align="center" valign="middle">'.$data->hoten.'</td>
			<td align="center" valign="middle">'.$data->diachi.'</td>
			<td align="center" valign="middle">'.$data->sdt.'</td>
            <td align="center" valign="middle">'.$data->madh.'</td>
            <td align="center" valign="middle">'.$data->ngaydat.'</td>
            <td align="center" valign="middle">'.$data->tongtien.'</td>
		  </tr>';	
		$dem++;
			
		}
		echo '</tbody>
			</table>';
		
	}
}

?>
-------
<?php 
include ("class/clsdocapi.php");
$p=new myapi();
?>
<!doctype html>
<html>
<head>
<meta charset="utf-8">
<title>Untitled Document</title>
</head>

<body>
<?php
$p->xuatdssv("http://localhost:89/gki_cnm7/api/xem.php");

?>
</body>
</html>
--------
public static void main(String[] args) {
       myform fm = new myform();
       fm.setVisible(true);
    }
-----
import java.net.HttpURLConnection;
import java.net.URL;
import java.nio.charset.Charset;
import org.json.JSONArray;
import org.json.JSONObject;
import org.json.JSONTokener;
import org.apache.commons.io.IOCase;
import org.apache.commons.io.IOUtils;
public class clsapi {
    public JSONArray docapi()
            {
                try
                {
                    String url="http://localhost:89/gki_cnm7/api/xem.php";
                    JSONArray jar = (JSONArray) new JSONTokener (IOUtils.toString(new URL(url).openStream(), Charset.forName("UTF-8"))).nextValue();
                    return jar;
                }catch (Exception e)
                {
                    System.out.println("loi ket noi");
                    return null;
                }
            }
    
}    
-------
import java.util.Vector;
import javax.swing.table.DefaultTableModel;
import org.json.JSONArray;
import org.json.JSONObject;
---------
private void loadtable()
    {
        try
        {
            clsapi cls = new clsapi();
            JSONArray jarr = new JSONArray();
            jarr = cls.docapi();
            Vector<Vector<String>> datalist=new Vector();
            for(int i=0;i<cls.docapi().length();i++)
            {
                JSONObject job= jarr.getJSONObject(i);
                Vector<String> data = new Vector<>();
                data.add(job.getString("makh"));
                data.add(job.getString("hoten"));
                data.add(job.getString("diachi"));
                data.add(job.getString("sdt"));
                data.add(job.getString("madh"));
                data.add(job.getString("ngaydat"));
                data.add(job.getString("tongtien"));
                datalist.add(data);
            }
            Vector<String> title = new Vector<>();
            title.add("Mã KH");
            title.add("HỌ TÊN");
            title.add("ĐỊA CHỈ");
            title.add("SĐT");
            title.add("MÃ KH");
            title.add("NGÀY ĐẶT");
            title.add("TỔNG TIỀN");
            DefaultTableModel model = new DefaultTableModel(datalist,title);
            tbl_kh.setModel(model);
        }catch (Exception e)
        {
        }
    }
-------
private void nut_loadActionPerformed(java.awt.event.ActionEvent evt) {                                         
        loadtable();// TODO add your handling code here:
    }  
