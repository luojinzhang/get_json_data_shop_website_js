# get_json_data_shop_website_js
Get json data from shop website

    
    var a = $(".p-cont.highlight > .p-list").children()
    var Loai = "";
    var Ma_Nhom_hang = 0;
    var Ma_so = "0";
    var listMonAn = [];
    var nhan_vien = 
    [
        {
          "Ho_ten": "Mập Văn Béo",
          "Ma_so": "NV_1",
          "Ten_dang_nhap": "NV_1",
          "Mat_khau": "NV_1"
        },
        {
          "Ho_ten": "Ốm Thị Gầy",
          "Ma_so": "NV_2",
          "Ten_dang_nhap": "NV_2",
          "Mat_khau": "NV_2"
        }
    ];

    function change_alias(alias) {
        var str = alias;
        str = str.toLowerCase();
        str = str.replace(/à|á|ạ|ả|ã|â|ầ|ấ|ậ|ẩ|ẫ|ă|ằ|ắ|ặ|ẳ|ẵ/g,"a"); 
        str = str.replace(/è|é|ẹ|ẻ|ẽ|ê|ề|ế|ệ|ể|ễ/g,"e"); 
        str = str.replace(/ì|í|ị|ỉ|ĩ/g,"i"); 
        str = str.replace(/ò|ó|ọ|ỏ|õ|ô|ồ|ố|ộ|ổ|ỗ|ơ|ờ|ớ|ợ|ở|ỡ/g,"o"); 
        str = str.replace(/ù|ú|ụ|ủ|ũ|ư|ừ|ứ|ự|ử|ữ/g,"u"); 
        str = str.replace(/ỳ|ý|ỵ|ỷ|ỹ/g,"y"); 
        str = str.replace(/đ/g,"d");
        str = str.replace(/!|@|%|\^|\*|\(|\)|\+|\=|\<|\>|\?|\/|,|\.|\:|\;|\'|\"|\&|\#|\[|\]|~|\$|_|`|-|{|}|\||\\/g," ");
        str = str.replace(/ + /g," ");
        str = str.replace(/ /g,"_");
        str = str.trim(); 
        return str.toUpperCase();
    }

    function random_Ngay()
    {
        var maxDate = Date.now();
        var minDate = Date.parse('1/1/2017');
        var randomDate = Math.floor(Math.random() * (maxDate - minDate) + minDate);
        return randomDate.toString();
    }
    function random_Don_gia_Nhap(price)
    {
        var price = parseInt(price)
        var profit = price > 500000 ? 100000 : price > 300000 ? 50000 : price > 100000 ? 25000 : price > 50000 ? 10000 : price > 20000 ? 5000 : 0;
        var price_Nhap_hang = price - profit;
        return price_Nhap_hang.toString();
    }
    function random_So_luong()
    {
        var so_luong = Math.floor(Math.random() * (100 - 1) + 1);
        return so_luong;
    }
    var saveData = (function () {
        var a = document.createElement("a");
        document.body.appendChild(a);
        a.style = "display: none";
        return function (data, fileName) {
            var json = JSON.stringify(data, null, 1),
                blob = new Blob([json], {type: "octet/stream"}),
                url = window.URL.createObjectURL(blob);
            a.href = url;
            a.download = fileName;
            a.click();
            window.URL.revokeObjectURL(url);
        };
    }());
    for (var i = 0; i < a.length; i++) {
        if (a[i].className == "p-list-cat") {
            if ( Loai !== a[i].textContent)
            {
                Ma_Nhom_hang++;
                Loai = a[i].textContent;
            }
        }
        else {
            for (var j = 0; j < a[i].children.length; j++) {
                var b = a[i].children[j].children;
                Ma_so = parseInt(Ma_so) + 1;
                var monAn = {
                    Ma_so: "",
                    Ten: "",
                    Don_gia_Ban: "",
                    Nhom_Mat_hang: {Ten: "" , Ma_so : ""},
                    Danh_sach_Xuat_hang: []
                }


                monAn.Ma_so = "MH_" + Ma_so.toString();
                monAn.Ten = $(b).find("figure > a").eq(0).attr("title")
                monAn.Don_gia_Ban= parseInt($(b).find(".price-box").eq(0).attr("data-price"))
                monAn.Nhom_Mat_hang.Ten = Loai;
                monAn.Nhom_Mat_hang.Ma_so = change_alias(Loai);

                var times = Math.floor(Math.random() * 4 + 1);
                for (var k = 0; k <= times; k++)
                {
                    var Danh_sach_Nhap_xuat = {
                        Ngay: "",
                        Don_gia: "",
                        So_luong: "",
                        Tien: "",
                        Nhan_vien: {Ho_ten: "", MSNV: ""}
                    };
                    Danh_sach_Nhap_xuat.Nhan_vien.Ho_ten = nhan_vien[Math.floor(Math.random() * 2 + 0)].Ho_ten;
                    Danh_sach_Nhap_xuat.Nhan_vien.MSNV = nhan_vien[Math.floor(Math.random() * 2 + 0)].Ma_so;
                    Danh_sach_Nhap_xuat.Ngay = random_Ngay();
                    Danh_sach_Nhap_xuat.Don_gia = monAn.Don_gia_Ban;
                    Danh_sach_Nhap_xuat.So_luong = random_So_luong();
                    Danh_sach_Nhap_xuat.Tien = parseInt(Danh_sach_Nhap_xuat.Don_gia) * parseInt(Danh_sach_Nhap_xuat.So_luong);

                    monAn.Danh_sach_Xuat_hang.push(Danh_sach_Nhap_xuat);
                }
                fileName = "MON_" + Ma_so + ".json";
                saveData(monAn, fileName);
                listMonAn.push(monAn);
            }
        }
    }
