---
title: "CF 102623H - Máy cắt cỏ"
description: "Trang trại là một mạng lưới nơi mỗi tế bào có tốc độ phát triển cỏ dại riêng. Một ô có giá trị a[i][j] nhận được nhiều đơn vị cỏ dại mỗi lúc."
date: "2026-08-04T17:20:28+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102623
codeforces_index: "H"
codeforces_contest_name: "2020 Lenovo Cup USST Campus Online Invitational Contest"
rating: 0
weight: 102623
solve_time_s: 109
verified: true
draft: false
---

[CF 102623H - Máy cắt cỏ](https://codeforces.com/problemset/problem/102623/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 49s 
**Đã xác minh:** có 

##Giải pháp 
#Hiểu vấn đề 

Trang trại là một mạng lưới nơi mỗi tế bào có tốc độ phát triển cỏ dại riêng. Một ô có giá trị`a[i][j]`thu được nhiều đơn vị cỏ dại mỗi lúc. Vào cuối những thời điểm nhất định, máy cắt sẽ dọn sạch toàn bộ một hàng hoặc toàn bộ cột và nhiệm vụ là tìm ra tổng số lượng cỏ dại đã bị loại bỏ trong tất cả các hoạt động. 

Đầu vào cung cấp lưới tốc độ tăng trưởng và sau đó là trình tự thời gian của các hoạt động xóa hàng và cột. Vì thời gian hoạt động ngày càng tăng nên mọi sự kiện đều có thể được xử lý theo thứ tự. Câu trả lời là tổng số cỏ dại được loại bỏ tại mỗi thời điểm phát quang, tính theo modulo`998244353`. 

Kích thước lưới tối đa là 500 x 500, do đó có tối đa 250000 ô. Số lượng hoạt động của máy cắt có thể lên tới 300000. Mô phỏng trực tiếp lưới điện sẽ yêu cầu chạm vào mọi ô sau mỗi hoạt động, trong trường hợp xấu nhất sẽ là khoảng 75000000000 hoạt động. Ngay cả việc lặp lại trên toàn bộ chiều cho mọi thao tác cũng chỉ được chấp nhận vì kích thước nhỏ và giải pháp dự định sử dụng thuộc tính này. 

Các giá trị lớn về tốc độ và thời gian tăng trưởng yêu cầu số học 64 bit trong quá trình tính toán. Một lỗi phổ biến là lưu trữ câu trả lời hoặc sản phẩm trung gian ở dạng số nguyên 32 bit. Một trường hợp tinh tế khác là khi một hàng và cột vừa bị cắt gần đây. Một ô không nhớ lần cuối cùng hàng của nó bị cắt và lần cuối cùng cột của nó bị cắt riêng. Số lượng cỏ dại thực tế của nó phụ thuộc vào thời điểm gần đây hơn trong hai thời điểm đó. 

Ví dụ, hãy xem xét:```
1 1 2
5
r 1 10
c 1 20
```Lần cắt đầu tiên loại bỏ`5 * 10 = 50`. Lần cắt thứ hai không xóa được gì vì ô duy nhất đã bị xóa ở thời điểm thứ 10 và phát triển thêm 10 giây nữa, vì vậy tổng số là`100`. 

Một giải pháp bất cẩn chỉ theo dõi lần cắt hàng cuối cùng hoặc chỉ theo dõi lần cắt cột cuối cùng sẽ tính không chính xác ô đang phát triển từ thời điểm 0 trong một trong các thao tác. 

Một trường hợp khác là tốc độ tăng trưởng bằng 0:```
1 1 1
0
r 1 100
```Câu trả lời là`0`. Việc triển khai vẫn phải cập nhật thời gian cắt ngay cả khi mức đóng góp bằng 0, vì các hoạt động trong tương lai phụ thuộc vào thời gian đó. 

# Phương pháp tiếp cận 

Một cách tiếp cận đơn giản là giữ lượng cỏ dại hiện tại trong mỗi ô. Khi thời gian trôi qua, hãy cộng phần tăng trưởng vào từng ô và khi một hàng hoặc cột bị cắt, hãy tính tổng dòng đó và xóa nó. Điều này tuân thủ chính xác quy trình và rất dễ chứng minh là đúng. Tuy nhiên, thời gian giữa các sự kiện có thể rất lớn và số lượng sự kiện là 300000. Việc cập nhật tất cả các ô sau mỗi sự kiện sẽ tốn phí`O(k*n*m)`, vượt xa giới hạn. 

Quan sát hữu ích là sự phát triển của một ô chỉ phụ thuộc vào lần cuối cùng hàng hoặc cột của nó bị xóa. Đối với một tế bào`(i,j)`, nếu việc cắt hàng cuối cùng xảy ra tại`R[i]`và việc cắt cột cuối cùng xảy ra vào lúc`C[j]`, thì cỏ dại hiện có là:`a[i][j] * (current_time - max(R[i], C[j]))`. 

Khi cắt một hàng, chúng ta chỉ cần tính công thức này cho các ô trong hàng đó. Điều tương tự cũng áp dụng cho việc cắt cột. Vì cả hai thứ nguyên tối đa là 500 nên việc kiểm tra toàn bộ một thứ nguyên cho mỗi sự kiện là khả thi. 

Lực lượng vũ phu hoạt động vì lưới nhỏ nhưng không thành công khi mô phỏng liên tục tất cả các ô. Việc quan sát về thời gian đặt lại hàng và cột độc lập cho phép chúng tôi giảm công việc xuống còn quét một hàng hoặc cột cho mỗi thao tác. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(k_n_m) | O(n*m) | Quá chậm | 
| Tối ưu | O(k*max(n,m)) | O(n*m) | Đã chấp nhận | 

#Hướng dẫn thuật toán 

1. Lưu trữ tốc độ tăng trưởng của tất cả các tế bào. Duy trì hai mảng:`last_row[i]`lưu trữ hàng khoảnh khắc cuối cùng`i`đã được xóa và`last_col[j]`lưu trữ cột khoảnh khắc cuối cùng`j`đã được xóa. Ban đầu cả hai đều bằng 0 vì lưới bắt đầu trống. 
2. Khi một hàng`x`được xóa vào thời điểm đó`t`, lặp qua từng cột`j`. Số tiền trong ô`(x,j)`được xác định bởi thời gian thanh toán bù trừ gần nhất giữa hàng`x`và cột`j`, vì vậy hãy thêm:`a[x][j] * (t - max(last_row[x], last_col[j]))`để trả lời. Sau khi xử lý hàng, đặt`last_row[x] = t`. 
3. Khi một cột`y`được xóa vào thời điểm đó`t`, thực hiện phép tính đối xứng trên mỗi hàng`i`:`a[i][y] * (t - max(last_row[i], last_col[y]))`Sau đó đặt`last_col[y] = t`. 
4. Trả lời theo modulo`998244353`sau khi bổ sung để giữ cho giá trị có thể quản lý được. 

Tại sao nó hoạt động: đối với mỗi ô, thuật toán sẽ thêm chính xác lượng cỏ dại tồn tại bất cứ khi nào ô đó bị xóa. Một ô chỉ bị ảnh hưởng bởi các thao tác trên hàng hoặc cột của chính nó. Giữa hai hoạt động như vậy, tế bào phát triển liên tục và điểm bắt đầu của khoảng thời gian tăng trưởng đó chính xác là thời điểm gần đây nhất trong hai lần làm sạch trước đó. Công thức được sử dụng trong mọi thao tác khớp với bất biến này, vì vậy mỗi đơn vị bị loại bỏ sẽ được tính một lần và chỉ một lần. 

#Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def solve():
    n, m, k = map(int, input().split())

    a = [list(map(int, input().split())) for _ in range(n)]

    last_row = [0] * n
    last_col = [0] * m

    ans = 0

    for _ in range(k):
        typ, x, t = input().split()
        x = int(x) - 1
        t = int(t)

        if typ == 'r':
            lr = last_row[x]
            row = a[x]
            for j in range(m):
                start = lr if lr > last_col[j] else last_col[j]
                ans += row[j] * (t - start)
            last_row[x] = t
        else:
            lc = last_col[x]
            for i in range(n):
                start = last_row[i] if last_row[i] > lc else lc
                ans += a[i][x] * (t - start)
            last_col[x] = t

        ans %= MOD

    print(ans % MOD)

if __name__ == "__main__":
    solve()
```Các mảng`last_row`Và`last_col`chứa trạng thái duy nhất cần thiết ngoài lưới ban đầu. Chúng đại diện cho thời gian đặt lại của mỗi dòng, do đó giá trị hiện tại của một ô không bao giờ cần được lưu trữ. 

Đối với thao tác hàng, biến cục bộ`lr`giữ dấu thời gian của hàng cũ trước khi cập nhật nó. Điều này quan trọng vì sự đóng góp phải sử dụng trạng thái trước khi máy cắt xóa hàng. Ý tưởng tương tự được sử dụng cho các cột có`lc`. 

Số nguyên Python tự động xử lý các tích lớn có giá trị lên tới`10^18`, nhưng phép toán modulo giữ cho câu trả lời cuối cùng bị chặn. Việc so sánh được viết thủ công thay vì sử dụng`max`bên trong các vòng lặp bên trong vì những vòng lặp này có thể thực thi hàng trăm triệu lần trong những trường hợp lớn nhất. 

# Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
2 2 3
1 2
3 4
r 1 5
c 2 6
r 1 7
```| Sự kiện | Thời gian dòng trước khi cập nhật | Thời gian dòng khác | Số tiền đã thêm | Trả lời | 
| --- | --- | --- | --- | --- | 
|`r 1 5`| hàng 1 = 0 | cột = 0,0 |`1*5 + 2*5 = 15`| 15 | 
|`c 2 6`| cột 2 = 0 | hàng = 5,0 |`2*(6-5) + 4*6 = 26`| 41 | 
|`r 1 7`| hàng 1 = 5 | cột = 0,6 |`1*(7-5) + 2*(7-6) = 4`| 45 | 

Dấu vết cho thấy lý do tại sao cần có dấu thời gian hàng và cột tối đa. Trong sự kiện thứ hai, ô`(1,2)`bắt đầu tăng từ thời điểm thứ 5 vì hàng của nó bị xóa muộn hơn cột của nó. 

Đối với mẫu thứ hai:```
3 4 1
1 2 3 4
5 6 7 8
9 10 11 12
r 1 1000000000000000000
```| Sự kiện | Dòng | Thời gian | Đóng góp | 
| --- | --- | --- | --- | 
|`r 1`| Hàng 1 | 10^18 |`(1+2+3+4)*10^18`| 

Kết quả được tính modulo`998244353`. Điều này chứng tỏ rằng thuật toán không bao giờ phụ thuộc vào kích thước của các giá trị thời gian mà chỉ phụ thuộc vào sự khác biệt giữa các dấu thời gian. 

# Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(k*max(n,m)) | Mỗi thao tác quét một hàng hoặc một cột. | 
| Không gian | O(n*m) | Các mảng lưới và dấu thời gian được lưu trữ. | 

Với kích thước giới hạn ở 500, quá trình quét cho mỗi thao tác bị giới hạn. Thuật toán tránh mọi sự phụ thuộc vào độ lớn của dấu thời gian, có thể đạt tới`10^18`. 

# Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    solve()
    out = sys.stdout.getvalue() if hasattr(sys.stdout, "getvalue") else ""
    sys.stdin = old
    return out

# sample 1
assert run("""2 2 3
1 2
3 4
r 1 5
c 2 6
r 1 7
""").strip() == "45"

# sample 2
assert run("""3 4 1
1 2 3 4
5 6 7 8
9 10 11 12
r 1 1000000000000000000
""").strip() == "172998509"

# single zero cell
assert run("""1 1 1
0
r 1 100
""").strip() == "0"

# row and column interaction
assert run("""1 1 2
5
r 1 10
c 1 20
""").strip() == "100"

# equal values
assert run("""2 2 2
7 7
7 7
r 1 3
c 1 5
""").strip() == "49"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Ô số 0 đơn | 0 | Xử lý tăng trưởng bằng không | 
| Một ô có hàng rồi cắt cột | 100 | Sử dụng đúng thời gian đặt lại mới nhất | 
| Giá trị lưới bằng nhau | 49 | Xử lý hàng và cột đối xứng | 

# Vỏ cạnh 

Ô bị ảnh hưởng bởi cả việc cắt hàng và cắt cột phải sử dụng thời gian đặt lại sau. TRONG:```
1 1 2
5
r 1 10
c 1 20
```hoạt động đầu tiên thêm`5 * 10 = 50`. Trước thao tác thứ hai, việc đặt lại hàng tại thời điểm 10 mới hơn lần đặt lại cột ban đầu, do đó đóng góp thứ hai là`5 * (20 - 10) = 50`. Thuật toán lưu trữ hai dấu thời gian này một cách riêng biệt và lấy mức tối đa của chúng. 

Khi một dòng bị xóa nhiều lần liên tiếp, thì lần xóa trước đó của dòng đó sẽ quan trọng. Ví dụ:```
1 1 2
3
r 1 4
r 1 9
```Sự kiện đầu tiên loại bỏ`12`, và cái thứ hai loại bỏ`15`. Thuật toán liên tục cập nhật`last_row`, do đó phép tính thứ hai bắt đầu từ thời điểm 4 thay vì thời gian 0. 

Khi giá trị thời gian cực kỳ lớn, chẳng hạn như`10^18`, không cần xử lý đặc biệt. Công thức chỉ thực hiện phép nhân và phép trừ trên các số nguyên Python, có thể biểu thị chính xác các giá trị này.
