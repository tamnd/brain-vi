---
title: "CF 103409C - Máy tự động điều hòa AC"
description: "Đặt $a{s+t-1}dots a1a0$ là biểu diễn nhị phân của một tổ hợp $(s,t)$, sao cho mỗi $ai trong {0,1}$ và $sum ai = t$. Phép quay của tiền tố có độ dài $j+1$ là phép biến đổi $$aj a{j-1}dots a0 ;leftarrow; a{j-1}chấm a0 aj,$$ với tất cả các chữ số khác không thay đổi."
date: "2026-07-03T11:52:30+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103409
codeforces_index: "C"
codeforces_contest_name: "The 2021 CCPC Guilin Onsite (XXII Open Cup, Grand Prix of EDG)"
rating: 0
weight: 103409
solve_time_s: 137
verified: false
draft: false
---

[CF 103409C - Máy tự động điều hòa AC](https://codeforces.com/problemset/problem/103409/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 17s 
**Đã xác minh:** không 

## Giải pháp 
## Thiết lập 

hãy để$a_{s+t-1}\dots a_1a_0$là biểu diễn nhị phân của một$(s,t)$-sự kết hợp, vì vậy mỗi$a_i \in {0,1}$Và$\sum a_i = t$. 

Xoay tiền tố có độ dài$j+1$là sự biến đổi$$a_j a_{j-1}\dots a_0 \;\leftarrow\; a_{j-1}\dots a_0 a_j,$$với tất cả các chữ số khác không thay đổi. 

Mục đích là để cho thấy rằng tất cả$(s,t)$-các tổ hợp có thể được tạo bằng cách xoay tiền tố liên tiếp ở dạng này và để rút ra chuỗi lệnh MMIX ánh xạ chuỗi bit$(a_{s+t-1}\dots a_0)_2$cho người kế nhiệm từ điển của nó khi$s+t<64$. 

## Giải pháp 

### (a) Tạo bằng các phép quay liên tiếp 

hãy để$x = a_{s+t-1}\dots a_1a_0$ở dạng nhị phân và diễn giải các kết hợp theo thứ tự từ điển của các chuỗi bit này. 

Cho phép$x$không phải là cuối cùng$(s,t)$-sự kết hợp. Viết$x$BẰNG$$x = u\,0\,1\,1^k\,0^\ell$$Ở đâu$u$là tiền tố dài nhất sao cho vị trí đầu tiên từ bên phải nơi có thể xảy ra thay đổi tại một mẫu$0,1$, Và$1^k 0^\ell$là hậu tố tối đa của các số ở cuối theo sau là các số 0 được xác định bởi điều kiện trọng số cố định. 

Tương tự, ở dạng kế thừa từ điển tiêu chuẩn, hãy để$j$là chỉ số lớn nhất sao cho$$a_j = 0,\quad a_{j-1} = 1.$$Như vậy$j$tồn tại trừ khi chuỗi đó$1^t0^s$, đó là sự kết hợp cuối cùng. 

Hãy để hậu tố phân hủy xung quanh$j$là$$x = a_{s+t-1}\dots a_{j+1}\,0\,1\,a_{j-2}\dots a_0.$$Cho phép$r$là số lượng$1$ở trong$a_{j-2}\dots a_0$. Người kế thừa từ điển học của$x$thu được bằng cách trao đổi$01 \to 10$tại các vị trí$j,j-1$và di chuyển phần còn lại$r$những cái càng xa càng tốt. Điều này tạo ra$$x' = a_{s+t-1}\dots a_{j+1}\,1\,0\,0^{s'}1^{r},$$cho một xác định duy nhất$s'$. 

Bây giờ hãy xem xét tiền tố$a_j a_{j-1}\dots a_0$. TRONG$x$tiền tố này có dạng$$a_j a_{j-1}\dots a_0 = 0\,1\,w,$$Ở đâu$w$chứa chính xác$r$những cái đó. 

Áp dụng phép quay với$j$:$$a_j a_{j-1}\dots a_0 \;\leftarrow\; a_{j-1}\dots a_0 a_j,$$vì vậy tiền tố trở thành$$1\,w\,0.$$Hậu tố$w$vẫn giữ nguyên thứ tự tương đối của nó, và đơn$0$di chuyển đến cuối khối quay. Hoạt động này di chuyển sự phân biệt$1$bên trái của cặp$01$vượt qua khối được xác định bởi$w$, khớp chính xác với việc phân phối lại những cái được yêu cầu bởi quy tắc kế thừa từ điển. 

Do đó, mỗi bước kế tiếp theo thứ tự từ điển tương ứng với một phép xoay tiền tố duy nhất được xác định bởi vị trí của mẫu có thể chấp nhận ngoài cùng bên phải$01$. Lặp lại quy tắc này từ$0^s1^t$đạt tới mọi$(s,t)$-kết hợp chính xác một lần, vì mỗi bước tạo ra từ kế tiếp theo từ điển và quá trình kết thúc tại$1^t0^s$. 

Điều này hoàn thành việc chứng minh. ∎ 

### (b) Triển khai MMIX của phiên bản kế nhiệm 

Hãy để chuỗi bit được lưu trữ trong sổ đăng ký$rA$, với$s+t<64$vì vậy các thao tác từ là hợp lệ. 

Danh tính tiêu chuẩn cho từ kế thừa từ điển của các từ nhị phân có số lượng cố định là:$$\text{next}(x) = r \;\;|\;\; \left(\frac{(r \oplus x) \gg 2}{c}\right),$$Ở đâu$$c = x \,\&\, (-x), \quad r = x + c.$$Điều này tương đương với phép biến đổi Gosper nổi tiếng để tạo ra số nguyên tiếp theo có cùng số$1$bit phù hợp$(s,t)$-các tổ hợp theo mã hóa nhị phân ở Mục 7.2.1.3. 

Trong MMIX, sử dụng các thanh ghi$rA$(đầu vào/đầu ra),$rB$,$rC$,$rD$:```
        SET     rB, rA
        NEG     rC, rA          % rC = -x
        AND     rC, rA, rC       % c = x & (-x)

        ADD     rD, rA, rC       % rD = r = x + c

        XOR     rB, rD, rA       % rB = r ^ x
        SR      rB, rB, 2        % rB = (r ^ x) >> 2
        DIV     rB, rB, rC       % rB = ((r ^ x) >> 2) / c

        OR      rA, rD, rB       % result = r | ...
```Đầu ra trong$rA$là phần tiếp theo từ điển của tổ hợp đầu vào bất cứ khi nào đầu vào không phải là đầu cuối. 

Tính chính xác xuất phát từ danh tính kế tiếp có số lượng dân số cố định tiêu chuẩn, duy trì số lượng$1$bit và tạo ra số nguyên lớn nhất nhỏ nhất, phù hợp với thứ tự từ điển của$(s,t)$-các tổ hợp theo mã hóa nhị phân (phương trình (2)-(4) trong Mục 7.2.1.3). 

## Xác minh 

Quy tắc xoay bảo toàn nhiều tập bit vì mỗi thao tác chỉ hoán vị các mục bên trong tiền tố, do đó số lượng$1$còn sót lại$t$. 

Mỗi bước sẽ tăng giá trị nhị phân theo thứ tự từ điển vì phép quay được chọn sẽ di chuyển một$1$từ vị trí thấp hơn ở vị trí ngoài cùng bên phải có thể chấp nhận được$01$mẫu lên vị trí cao hơn trong khi đẩy các số 0 sang trái, khớp với phép biến đổi kế tiếp trên chuỗi nhị phân có trọng số cố định. 

Chuỗi MMIX chỉ sử dụng các nhận dạng số học thuận nghịch cho$c = x & (-x)$và phân rã đại số$x = r - c$, đảm bảo tất cả các giá trị trung gian vẫn được xác định rõ ràng trong mô hình thanh ghi 64 bit khi$s+t<64$. 

## Ghi chú 

Việc giải thích phép quay tương ứng với một dạng tuần hoàn của việc tạo ra các tổ hợp cửa quay, trong đó mỗi quá trình chuyển đổi sẽ di chuyển một$1$thông qua một chu trình tiền tố được kiểm soát thay vì thực hiện sắp xếp lại toàn cục. 

Công thức MMIX là phép tính “kết hợp tiếp theo” cổ điển của Gosper được điều chỉnh phù hợp với mô hình máy của Knuth và nó trùng khớp với phần tiếp theo từ điển trên mã hóa nhị phân được sử dụng trong Phần 7.2.1.3.
