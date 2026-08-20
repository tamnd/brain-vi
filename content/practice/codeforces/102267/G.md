---
title: "CF 102267G - Chế độ ăn kiêng"
description: "Chúng tôi có một dãy phòng được đặt hàng. Mỗi phòng có người i chứa một bệnh nhân được mô tả bởi a[i] và b[i]. Một robot bắt đầu với x đơn vị thức ăn và thăm các phòng từ trái sang phải, bỏ qua các phòng có bệnh nhân đã chết."
date: "2026-08-19T03:36:55+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102267
codeforces_index: "G"
codeforces_contest_name: "The 2019 University of Jordan Collegiate Programming Contest"
rating: 0
weight: 102267
solve_time_s: 997
verified: false
draft: false
---

[CF 102267G - Chế độ ăn kiêng](https://codeforces.com/problemset/problem/102267/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 16 phút 37 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một dãy phòng được đặt hàng. Mỗi phòng chiếm dụng`i`chứa một bệnh nhân được mô tả bởi`a[i]`Và`b[i]`. Một robot bắt đầu với`x`đơn vị thức ăn và thăm các phòng từ trái sang phải, bỏ qua các phòng có bệnh nhân đã chết. Đối với bệnh nhân còn sống, robot có thể cung cấp cho họ`a[i]`chỉ thực phẩm nếu ít nhất`a[i]`thức ăn còn lại. Sau khi nhận được thức ăn, bệnh nhân sẽ chết nếu robot vẫn còn nhiều hơn`b[i]`đồ ăn. 

Truy vấn loại 1 yêu cầu hai con số: có bao nhiêu bệnh nhân hiện đang sống chết trong chuyến đi đó và bao nhiêu bệnh nhân còn sống không bao giờ được tiếp cận vì robot hết thức ăn. Những bệnh nhân chết sẽ bị loại bỏ vĩnh viễn khỏi quá trình này. Truy vấn loại 2 thay thế bệnh nhân trong một phòng bằng một bệnh nhân mới, ngay cả khi phòng đó trước đó trở nên trống vì bệnh nhân đã chết. 

Chi tiết quan trọng là truy vấn loại 1 thay đổi cấu trúc dữ liệu. Một bệnh nhân chết sẽ vắng mặt trong mỗi chuyến đi sau đó cho đến khi truy vấn loại 2 đưa một bệnh nhân mới vào phòng đó. Điều này ngăn chúng tôi xử lý mọi truy vấn dưới dạng mô phỏng độc lập. 

Với tối đa`10^5`phòng và`10^5`truy vấn, một`O(n)`mô phỏng cho mọi truy vấn loại 1 có thể thực hiện khoảng`10^10`hoạt động của bệnh nhân trong trường hợp xấu nhất. Điều đó vượt xa giới hạn 2 giây cho phép. Các giá trị của`a`,`b`, Và`x`cũng đạt được`10^18`, vì vậy mọi tổng phải sử dụng số nguyên 64 bit. Số nguyên Python đã có độ chính xác tùy ý, do đó không có vấn đề tràn trong quá trình triển khai. 

Có một số trường hợp ranh giới rất dễ bị xử lý sai. Đầu tiên, cái chết sử dụng một sự bất bình đẳng nghiêm ngặt. Ví dụ,```
15 511 10
```Bệnh nhân nhận`5`, rời đi`5`, đó chính xác là giới hạn an toàn của họ. Đầu ra là```
0 0
```Việc thực hiện bất cẩn bằng cách sử dụng`>= b[i]`thay vì`> b[i]`sẽ giết nhầm bệnh nhân. 

Thứ hai, bệnh nhân không được ăn thì chưa chết. Ví dụ,```
15 10011 4
```Robot không thể đưa ra những yêu cầu`5`, vì vậy bệnh nhân sống sót và đơn giản là không nhận được thức ăn. Đầu ra là```
0 1
```Thứ ba, phòng chết sẽ bị bỏ qua trong những chuyến đi sau này. Ví dụ,```
15 021 61 1
```Truy vấn đầu tiên giết chết bệnh nhân vì một đơn vị vẫn còn và`1 > 0`. Căn phòng sau đó trống rỗng. Truy vấn thứ hai không thể giết bất cứ ai và cũng không có bệnh nhân nào để nuôi, vì vậy kết quả đầu ra là```
1 00 0
```Cuối cùng, một bản cập nhật có thể khôi phục lại phòng chết. Ví dụ,```
15 021 62 2 10 1
```Truy vấn đầu tiên giết chết bệnh nhân ban đầu. Truy vấn thứ hai sẽ chèn một bệnh nhân mới vào cùng phòng đó. Việc coi phòng chết là vĩnh viễn không thể sử dụng được sẽ làm mất bệnh nhân mới. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là giữ bệnh nhân hiện tại ở mỗi phòng và mô phỏng robot từ phòng`1`xuyên qua phòng`n`cho mọi truy vấn loại 1. Tại mỗi phòng đến thăm chúng tôi trừ đi`a[i]`, kiểm tra xem bệnh nhân có chết hay không và đánh dấu bệnh nhân đã chết là vắng mặt. Điều này đúng vì nó tuân thủ chính xác quy trình, bao gồm cả việc bệnh nhân đã chết sẽ biến mất khỏi những chuyến đi sau này. 

Vấn đề là trường hợp xấu nhất. Giả sử có`10^5`Rooms và gần như mọi truy vấn đều là truy vấn loại 1 đối với một lượng thức ăn đủ lớn. Sau đó, một truy vấn sẽ kiểm tra`10^5`phòng, và`10^5`truy vấn có thể yêu cầu đại khái`10^10`hoạt động. Mặc dù mỗi mô phỏng riêng lẻ đều đơn giản nhưng tổng số lại quá lớn. 

Quan sát quan trọng là ngừng nghĩ về một bệnh nhân tại một thời điểm và thay vào đó mô tả toàn bộ chuỗi còn lại bằng ba giá trị. 

Đối với một nhóm bệnh nhân còn sống liên tiếp, hãy`sum`là tổng lượng thức ăn mà những bệnh nhân đó yêu cầu. Cho phép`cnt`là số của họ. Điều quan trọng nhất là hãy`mn`là giá trị nhỏ nhất của`food already consumed before this patient + a[i] + b[i]`. 

Nếu robot bắt đầu nhóm với số tiền bổ sung`s`lượng thực phẩm đã được tiêu thụ thì bệnh nhân sẽ chết chính xác khi giá trị này nhỏ hơn`x`. Vì vậy, nhóm này có một bệnh nhân chết đúng vào thời điểm`mn + s < x`. 

Thuộc tính này kết hợp một cách tự nhiên khi hai đoạn liên tiếp được nối với nhau. Đoạn bên trái đóng góp mức tối thiểu của riêng nó. Đối với phân khúc bên phải, mọi tiền tố bắt đầu sau khi tất cả thực phẩm mà phân khúc bên trái yêu cầu đã được tiêu thụ, vì vậy mọi ứng cử viên bên phải sẽ được dịch chuyển theo`left.sum`. 

Điều đó mang lại một cây phân đoạn có nút lưu trữ`cnt`,`sum`, Và`mn`. 

Bản cập nhật loại 2 chỉ thay đổi một lá, do đó việc xây dựng lại cây phân đoạn sẽ mất`O(log n)`thời gian. 

Truy vấn loại 1 thú vị hơn. Chúng tôi tìm kiếm đệ quy mọi bệnh nhân thỏa mãn điều kiện tử vong và loại bỏ những chiếc lá đó. Khi toàn bộ phân khúc có`mn + consumed >= x`, không thể có cái chết ở bất kỳ đâu bên trong nó, vì vậy toàn bộ phân đoạn có thể bị bỏ qua. Mỗi chiếc lá bị loại bỏ theo cách này sẽ chết vĩnh viễn cho đến khi có bản cập nhật tạo lại một bệnh nhân ở đó. Do đó, trên toàn bộ đầu vào, phần đắt tiền của tất cả các truy vấn loại 1 chỉ có thể truy cập từng bệnh nhân dưới dạng lá chết một lần giữa các lần chèn. 

Sau khi loại bỏ những bệnh nhân đã chết, chúng tôi cần số lượng chưa nhận được thức ăn. Vì mọi`a[i]`là tích cực, thực phẩm tích lũy mà bệnh nhân còn sống tiêu thụ đang tăng lên nghiêm trọng. Chúng ta có thể đi xuống cùng một cây phân đoạn để tìm xem có bao nhiêu bệnh nhân còn sống có nhu cầu tích lũy nhiều nhất`x`. Những bệnh nhân còn sống còn lại không nhận được thức ăn. 

Đây chính là cấu trúc cây phân đoạn được sử dụng trong giải pháp cuộc thi chính thức. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(nq)`|`O(n)`| Quá chậm | 
| Tối ưu |`O((n + q) log n)`khấu hao |`O(n)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng cây phân đoạn trên các phòng. Đối với một bệnh nhân còn sống`(a, b)`, cửa hàng`cnt = 1`,`sum = a`, Và`mn = a + b`. Một căn phòng trống cửa hàng`cnt = 0`,`sum = 0`, Và`mn = infinity`. 

Đối với hai nút liên tiếp`L`Và`R`, giá trị tổng hợp của chúng là`cnt = L.cnt + R.cnt`

`sum = L.sum + R.sum`

`mn = min(L.mn, L.sum + R.mn)`. 

Ứng viên thứ hai có`L.sum`được thêm vào vì mọi bệnh nhân ở trẻ bên phải chỉ được tiếp cận sau khi tất cả bệnh nhân còn sống ở trẻ bên trái đã nhận được thức ăn của họ. 
2. Đối với truy vấn loại 2`(a, b, c)`, thay phòng`c`với một chiếc lá chứa`cnt = 1`,`sum = a`, Và`mn = a + b`. 

Điều này xử lý cả hai trường hợp thống nhất. Nếu bệnh nhân cũ còn sống, họ sẽ được thay thế. Nếu bệnh nhân cũ đã chết, chiếc lá trống trước đó sẽ lại bị chiếm giữ. 
3. Đối với truy vấn loại 1 về lượng thức ăn`x`, ghi nhớ số lượng bệnh nhân còn sống được lưu trữ tại gốc. 

Sau đó loại bỏ đệ quy mọi bệnh nhân còn sống có điều kiện tử vong được thỏa mãn. Đối với một đoạn bắt đầu sau`used`thực phẩm đã được tiêu thụ, ứng cử viên tử vong tối thiểu của nó là`mn + used`. Nếu điều này ít nhất`x`, không có bệnh nhân nào trong toàn bộ phân đoạn đó chết nên chúng tôi dừng ngay lập tức. 
4. Khi đệ quy đến một lá có ứng cử viên nhỏ hơn`x`, hãy loại bỏ bệnh nhân đó bằng cách đặt số lượng và yêu cầu thực phẩm về 0 và mức tối thiểu là vô cùng. 

Sự so sánh chặt chẽ là cần thiết. Bệnh nhân chỉ tử vong khi lượng thức ăn còn lại nhiều hơn`b`, vì vậy bình đẳng có nghĩa là sinh tồn. 
5. Sau khi xóa pass, hãy trừ số gốc mới khỏi số gốc cũ. Sự khác biệt này chính xác là số lượng bệnh nhân đã chết trong truy vấn này. 
6. Đếm xem có bao nhiêu bệnh nhân còn sống thực sự có thể nhận được thức ăn. Bắt đầu từ gốc với`used = 0`, kiểm tra đứa trẻ bên trái. Nếu như`used + left.sum <= x`, mọi bệnh nhân còn sống trong đứa trẻ đó đều có thể được phục vụ, vì vậy hãy thêm`left.cnt`đến câu trả lời và chuyển sang đứa trẻ bên phải với`used`tăng lên bởi`left.sum`. Ngược lại, tiếp tục vào con bên trái. 

Tích cực`a[i]`các giá trị làm cho nhu cầu tích lũy tăng lên đáng kể, do đó chỉ cần khám phá một đường dẫn từ gốc đến lá. 
7. Số bệnh nhân còn sống không được nhận thức ăn là số bệnh nhân còn lại sau khi qua đời trừ đi số lượng có thể phục vụ. In ra số người chết theo sau là số lượng không được giám sát. 

### Tại sao nó hoạt động 

Điều bất biến là mỗi nút cây phân đoạn mô tả chính xác các bệnh nhân hiện đang sống trong khoảng của nó, theo thứ tự phòng ban đầu của họ. Của nó`sum`là tổng nhu cầu thực phẩm của họ, trong khi`mn`là giá trị nhỏ nhất của`consumed_before + a[i] + b[i]`trên những bệnh nhân đó. 

Đối với bất kỳ bệnh nhân nào,`a[i] + b[i]`được so sánh với thực phẩm được tiêu thụ bởi bệnh nhân đó. Như vậy bệnh nhân chết đúng lúc`consumed_before + a[i] + b[i] < x`, đó chính xác là điều kiện được biểu thị bởi`mn + used < x`cho một phân khúc. Một phân đoạn có mức tối thiểu không đạt được điều kiện này không chứa bệnh nhân đã chết và có thể được bỏ qua một cách an toàn. Mỗi phân đoạn được khám phá để hướng tới một chiếc lá đủ điều kiện cuối cùng sẽ loại bỏ chính xác những bệnh nhân thỏa mãn điều kiện tử vong. 

Sau khi tất cả những bệnh nhân đó được loại bỏ, những bệnh nhân còn lại`sum`các giá trị chỉ mô tả bệnh nhân còn sống. Vì mọi nhu cầu đều dương nên nhu cầu tích lũy là đơn điệu, nên lần duyệt thứ hai sẽ tính chính xác số bệnh nhân còn sống có thể nhận được thức ăn. Trừ con số đó khỏi dân số còn lại sẽ cho ra chính xác những bệnh nhân không nhận được gì. 

## Giải pháp Python```python
Pythonimport sysinput = sys.stdin.readline
INF = 10**30

def solve():    data = list(map(int, sys.stdin.buffer.read().split()))    it = iter(data)
    n = next(it)
    a = [0] * n    b = [0] * n
    for i in range(n):        a[i] = next(it)        b[i] = next(it)
    size = 4 * n + 5    cnt = [0] * size    sm = [0] * size    mn = [INF] * size
    def pull(v):        lc = v * 2        rc = lc + 1
        cnt[v] = cnt[lc] + cnt[rc]        sm[v] = sm[lc] + sm[rc]        mn[v] = min(mn[lc], sm[lc] + mn[rc])
    def build(v, l, r):        if l == r:            cnt[v] = 1            sm[v] = a[l]            mn[v] = a[l] + b[l]            return
        m = (l + r) // 2        build(v * 2, l, m)        build(v * 2 + 1, m + 1, r)        pull(v)
    def update(v, l, r, pos, na, nb):        if l == r:            cnt[v] = 1            sm[v] = na            mn[v] = na + nb            return
        m = (l + r) // 2        if pos <= m:            update(v * 2, l, m, pos, na, nb)        else:            update(v * 2 + 1, m + 1, r, pos, na, nb)
        pull(v)
    def kill(v, l, r, x, used):        if mn[v] + used >= x:            return
        if l == r:            cnt[v] = 0            sm[v] = 0            mn[v] = INF            return
        m = (l + r) // 2        lc = v * 2        rc = lc + 1
        if mn[lc] + used < x:            kill(lc, l, m, x, used)
        if mn[rc] + used + sm[lc] < x:            kill(rc, m + 1, r, x, used + sm[lc])
        pull(v)
    def served(v, l, r, x, used):        if l == r:            return cnt[v] if used + sm[v] <= x else 0
        m = (l + r) // 2        lc = v * 2        rc = lc + 1
        if used + sm[lc] <= x:            return cnt[lc] + served(                rc, m + 1, r, x, used + sm[lc]            )
        return served(lc, l, m, x, used)
    build(1, 0, n - 1)
    q = next(it)    out = []
    for _ in range(q):        typ = next(it)
        if typ == 1:            x = next(it)
            before = cnt[1]            kill(1, 0, n - 1, x, 0)            after = cnt[1]
            dead = before - after            fed = served(1, 0, n - 1, x, 0)            hungry = after - fed
            out.append(f"{dead} {hungry}")
        else:            na = next(it)            nb = next(it)            c = next(it) - 1
            update(1, 0, n - 1, c, na, nb)
    sys.stdout.write("\n".join(out))

if __name__ == "__main__":    solve()
```Ba mảng`cnt`,`sm`, Và`mn`là toàn bộ trạng thái của nút cây phân đoạn.`cnt`cho chúng ta biết còn lại bao nhiêu bệnh nhân còn sống,`sm`cho chúng ta biết họ tiêu thụ chung bao nhiêu thực phẩm, và`mn`xác định tình trạng tử vong sớm nhất có thể xảy ra trong phân khúc. 

Hoạt động hợp nhất là trung tâm của việc thực hiện. Mức tối thiểu bên trái đã được đo từ đầu phân khúc kết hợp. Mỗi ứng cử viên ở đứa trẻ bên phải cần toàn bộ số tiền bên trái được cộng vào lượng thực phẩm tiêu thụ của nó, điều này mang lại`sm[left] + mn[right]`. 

các`kill`chức năng nhận được`used`, thực phẩm đã được tiêu thụ trước phân khúc hiện tại. Tại một chiếc lá,`mn + used < x`nghĩa là bệnh nhân chết. Tại một nút bên trong, thử nghiệm tương tự cho phép chúng ta bỏ qua toàn bộ nút nếu không có khả năng tử vong. Đúng đứa trẻ nhận được`used + sm[left]`, bởi vì để đạt được nó đòi hỏi phải tiêu thụ tất cả bệnh nhân còn sống ở đứa trẻ bên trái trước tiên. 

Chức năng cập nhật luôn tạo ra một chiếc lá sống. Điều này là có chủ ý vì truy vấn loại 2 sẽ chèn bệnh nhân mới ngay cả khi phòng trước đây đã có người đã chết. 

các`served`công dụng chức năng`<= x`, không`< x`. Bệnh nhân có thể nhận được thức ăn khi còn lại chính xác. Bởi vì tất cả`a[i]`có ít nhất một, nhu cầu tích lũy ngày càng tăng ở những bệnh nhân còn sống, điều này làm giảm hoạt động này xuống còn một con đường cây. 

Không cần loại 64 bit rõ ràng trong Python. Tổng tích lũy lớn nhất có thể là khoảng`10^14`và số nguyên Python vẫn biểu diễn các giá trị vượt quá mức đó một cách an toàn.`INF`chỉ cần lớn hơn mọi ngưỡng tử vong có ý nghĩa có thể có. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Ban đầu năm bệnh nhân có nhu cầu thực phẩm tích lũy`1, 6, 109, 110, 115`. Ứng cử viên tử vong của họ là`11, 18, 179, 111, 118`. 

| Hoạt động | Đồ ăn`x`| Sống trước | Tửu thí sinh dưới đây`x`| Sống sau | Fed | Không phục vụ | 
| --- | --- | --- | --- | --- | --- | --- | 
|`1 400`| 400 | 5 | cả năm | 0 | 0 | 0 | 
|`2 3 13 3`| | 0 | | 1 | | | 
|`2 5 3 1`| | 1 | | 2 | | | 
|`1 3`| 3 | 2 | không | 2 | 0 | 2 | 

Truy vấn đầu tiên giết chết mọi bệnh nhân vì mọi ứng cử viên tử vong đều ở dưới`400`. Do đó, tất cả năm phòng đều trở nên trống rỗng. Bản cập nhật đầu tiên sẽ đưa một bệnh nhân vào phòng 3 và bản cập nhật thứ hai sẽ đưa một bệnh nhân khác vào phòng 1. 

Đối với câu hỏi cuối cùng, bệnh nhân ở phòng 1 yêu cầu`5`thức ăn nên robot sẽ dừng ngay lập tức khi chỉ`3`. Bệnh nhân ở phòng 3 không bao giờ được tiếp cận. Không có bệnh nhân nào chết, đưa ra`0 2`. 

### Mẫu 2 

Những bệnh nhân ban đầu có`a`giá trị`1, 2, 3`Và`b`giá trị`2, 3, 4`. 

| Hoạt động | Đồ ăn`x`| Sống trước | Ứng cử viên tử vong | Cái chết | Sống sau | Fed | Không phục vụ | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
|`1 6`| 6 | 3 |`3`| 1 | 2 | 2 | 0 | 
|`1 13`| 13 | 2 |`5, 9`| 2 | 0 | 0 | 0 | 
|`2 1 1 1`| | 0 | | | 1 | | | 
|`2 2 4 2`| | 1 | | | 2 | | | 
|`1 20`| 20 | 2 |`2, 7`| 2 | 0 | 0 | 0 | 

Vì`x = 6`, bệnh nhân đầu tiên có nguy cơ tử vong`1 + 2 = 3`, thế là họ chết. Hai người còn lại sống sót vì ứng cử viên của họ là`6`Và`10`. Cả hai đều có thể được cho ăn. 

Vì`x = 13`, hai bệnh nhân còn lại có ứng viên`5`Và`9`, thế là cả hai đều chết. Hai bản cập nhật sau đây tạo lại phòng 1 và 2 với những bệnh nhân mới. Truy vấn cuối cùng giết chết cả hai bệnh nhân mới. 

Dấu vết cho thấy tại sao cây phải thay đổi sau mỗi truy vấn loại 1. Truy vấn thứ hai không được đánh giá dựa trên ba bệnh nhân ban đầu và truy vấn cuối cùng được đánh giá dựa trên hai bệnh nhân được chèn vào bởi các bản cập nhật. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O((n + q) log n)`khấu hao | Tòa nhà mất`O(n)`, mỗi lần cập nhật sẽ mất`O(log n)`, mỗi lần tìm kiếm phục vụ sẽ mất`O(log n)`, và mỗi cái chết sẽ loại bỏ một lá, vì vậy tất cả các lần xóa cùng nhau đều có giá`O((n + q) log n)`. | 
| Không gian |`O(n)`| Cây phân đoạn chứa`O(n)`các nút và ba mảng số nguyên được lưu trữ. | 

Khoản khấu hao quan trọng đến từ tính chất phá hoại của truy vấn loại 1. Một bệnh nhân chỉ có thể được xóa một lần trước khi một truy vấn loại 2 khác chèn vào thay thế. Do đó, mặc dù một truy vấn xóa có thể kiểm tra nhiều nút cây, tổng số lần xóa lá trên tất cả các truy vấn bị giới hạn bởi số lượng bệnh nhân ban đầu cộng với số lần chèn. Điều này giữ cho giải pháp trong`10^5`quy mô mà bài toán yêu cầu. 

## Trường hợp thử nghiệm```python
Pythonimport sysimport io

def solve(data: str) -> str:    tokens = list(map(int, data.split()))    it = iter(tokens)
    n = next(it)
    a = [0] * n    b = [0] * n
    for i in range(n):        a[i] = next(it)        b[i] = next(it)
    INF = 10**30    size = 4 * n + 5
    cnt = [0] * size    sm = [0] * size    mn = [INF] * size
    def pull(v):        lc = v * 2        rc = lc + 1        cnt[v] = cnt[lc] + cnt[rc]        sm[v] = sm[lc] + sm[rc]        mn[v] = min(mn[lc], sm[lc] + mn[rc])
    def build(v, l, r):        if l == r:            cnt[v] = 1            sm[v] = a[l]            mn[v] = a[l] + b[l]            return        m = (l + r) // 2        build(v * 2, l, m)        build(v * 2 + 1, m + 1, r)        pull(v)
    def update(v, l, r, pos, na, nb):        if l == r:            cnt[v] = 1            sm[v] = na            mn[v] = na + nb            return        m = (l + r) // 2        if pos <= m:            update(v * 2, l, m, pos, na, nb)        else:            update(v * 2 + 1, m + 1, r, pos, na, nb)        pull(v)
    def kill(v, l, r, x, used):        if mn[v] + used >= x:            return
        if l == r:            cnt[v] = 0            sm[v] = 0            mn[v] = INF            return
        m = (l + r) // 2        lc = v * 2        rc = lc + 1
        if mn[lc] + used < x:            kill(lc, l, m, x, used)
        if mn[rc] + used + sm[lc] < x:            kill(rc, m + 1, r, x, used + sm[lc])
        pull(v)
    def served(v, l, r, x, used):        if l == r:            return cnt[v] if used + sm[v] <= x else 0
        m = (l + r) // 2        lc = v * 2        rc = lc + 1
        if used + sm[lc] <= x:            return cnt[lc] + served(                rc, m + 1, r, x, used + sm[lc]            )
        return served(lc, l, m, x, used)
    build(1, 0, n - 1)
    q = next(it)    ans = []
    for _ in range(q):        typ = next(it)
        if typ == 1:            x = next(it)
            before = cnt[1]            kill(1, 0, n - 1, x, 0)            after = cnt[1]
            dead = before - after            fed = served(1, 0, n - 1, x, 0)            hungry = after - fed
            ans.append(f"{dead} {hungry}")        else:            na = next(it)            nb = next(it)            c = next(it) - 1            update(1, 0, n - 1, c, na, nb)
    return "\n".join(ans)

sample1 = """\51 105 12103 701 15 341 4002 3 13 32 5 3 11 3"""
assert solve(sample1) == """\5 00 2""", "sample 1"

sample2 = """\31 22 33 451 61 132 1 1 12 2 4 21 20"""
assert solve(sample2) == """\1 02 02 0""", "sample 2"

minimum_case = """\15 541 51 42 2 10 11 2"""
assert solve(minimum_case) == """\0 00 10 0""", "minimum size and exact equality"

reinsert_dead = """\15 031 62 2 10 11 2"""
assert solve(reinsert_dead) == """\1 00 0""", "dead room can be reused"

all_equal = """\42 12 12 12 131 82 2 1 21 5"""
assert solve(all_equal) == """\4 02 0""", "all equal values"

boundary_case = """\32 03 14 041 21 52 1 100 11 1"""
assert solve(boundary_case) == """\0 21 00 1""", "exact food and strict death boundary"

# Maximum-size structural test.# Every patient has a=1, b=10^18, so no patient dies for x=10^18.# The query feeds all 100000 patients.n = 100000max_input = [str(n)]max_input.extend(["1 1000000000000000000"] * n)max_input.append("1")max_input.append("1 100000")
max_output = solve("\n".join(max_input))assert max_output == "0 0", "maximum n"

print("all tests passed")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 5 5 / 1 5 / ...`|`0 0`| Kích thước tối thiểu và sự bình đẳng ở ranh giới tử vong | 
|`1 / 5 0 / 1 6 / 2 2 10 1 / ...`|`1 0`sau đó`0 0`| Một phòng chết có thể nhận được một sự thay thế | 
| Bốn bệnh nhân giống hệt nhau`(2, 1)`giá trị |`4 0`sau đó`2 0`| Xóa nhiều lần và giá trị bằng nhau | 
| Ba bệnh nhân được lựa chọn cẩn thận`x`giá trị |`0 2`,`1 0`,`0 1`| Ranh giới thực phẩm chính xác và sự bất bình đẳng về tử vong nghiêm ngặt | 
| Đã tạo`n = 100000`đầu vào |`0 0`| Đầu vào có kích thước tối đa và`O(n)`xây dựng | 

## Vỏ cạnh 

Ranh giới tử vong nghiêm ngặt được xử lý trực tiếp bằng điều kiện`mn + used < x`. Coi như```
15 511 10
```Cửa hàng lá`mn = 5 + 5 = 10`. Từ`10 < 10`là sai,`kill`khiến bệnh nhân còn sống. Việc truyền tải phục vụ nhìn thấy`sum = 5 <= 10`, thế là bệnh nhân được cho ăn. Đầu ra là`0 0`. Một so sánh không nghiêm ngặt sẽ xóa bệnh nhân một cách không chính xác. 

Một bệnh nhân không đủ khả năng chi trả cho bữa ăn của mình sẽ được xử lý tách biệt khỏi cái chết. Vì```
15 10011 4
```chiếc lá có`mn = 105`, nên không có cái chết xảy ra. Kiểm tra truyền tải phục vụ`5 <= 4`, điều này là sai và báo cáo một bệnh nhân còn sống không được phục vụ. Đầu ra là`0 1`. 

Bệnh nhân chết biến mất khỏi cây đoạn`cnt`Và`sum`. Vì```
15 021 61 1
```truy vấn đầu tiên có`mn = 5`, Và`5 < 6`, do đó chiếc lá trở nên trống rỗng. Gốc sau đó có`cnt = 0`Và`sum = 0`. Truy vấn thứ hai nhìn thấy một cây trống, vì vậy cả số người chết và số người không được giám sát đều bằng 0. 

Một bản cập nhật khôi phục phòng chết bằng cách viết một chiếc lá hoàn toàn mới. Vì```
15 031 62 2 10 11 2
```truy vấn đầu tiên loại bỏ bản gốc`(5, 0)`kiên nhẫn. Bản cập nhật sau đó thay thế lá trống đó bằng`(2, 10)`, ứng cử viên tử vong của ai là`12`. Với`x = 2`, ứng viên không ở dưới`2`và bệnh nhân chính xác có thể đủ khả năng chi trả theo yêu cầu của họ`2`đồ ăn. Đầu ra cuối cùng là`0 0`. 

Trường hợp hoàn toàn bằng nhau kiểm tra xem việc xóa đệ quy có xử lý chính xác một số lá liên tiếp hay không. Với```
42 12 12 12 111 8
```các ứng cử viên cái chết là`3, 5, 7, 9`. Ba cái đầu tiên ở dưới đây`8`, vậy có đúng ba bệnh nhân chết, còn người thứ tư sống sót. Cây phân đoạn có thể tỉa bất kỳ cây con nào có giá trị tối thiểu ít nhất là`8`, và nó loại bỏ chính xác các lá đủ điều kiện. 

Trường hợp kích thước tối đa sử dụng`100000`phòng và một truy vấn duy nhất với đủ thức ăn để nuôi tất cả mọi người. Mọi`a`là`1`, do đó nhu cầu tích lũy đạt chính xác`100000`, trong khi mọi`b`là`10^18`. Không có bệnh nhân nào chết, và mọi bệnh nhân đều được phục vụ. Đầu ra là`0 0`. Điều này xác nhận cả dấu vết bộ nhớ và thực tế là cây phân đoạn không thực hiện công việc không cần thiết cho mỗi bệnh nhân khi không có khả năng tử vong.
