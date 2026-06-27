---
title: "CF 105079A - Đặt bánh nướng nhỏ"
description: "Chúng tôi được tổ chức một bữa tiệc với số lượng khách cố định và mỗi vị khách nêu tên chính xác một hương vị bánh cupcake mà họ sẽ hài lòng. Mỗi hương vị được xác định bởi một số nguyên từ 1 đến M."
date: "2026-06-27T22:48:10+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105079
codeforces_index: "A"
codeforces_contest_name: "UTPC x WiCS Contest 04-05-23 (UT Internal)"
rating: 0
weight: 105079
solve_time_s: 71
verified: false
draft: false
---

[CF 105079A - Đặt bánh nướng nhỏ](https://codeforces.com/problemset/problem/105079/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 11 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được tổ chức một bữa tiệc với số lượng khách cố định và mỗi vị khách nêu tên chính xác một hương vị bánh cupcake mà họ sẽ hài lòng. Mỗi hương vị được xác định bằng một số nguyên từ 1 đến M. Alice chỉ có thể mua bánh nướng nhỏ theo bó 12 chiếc, nghĩa là nếu cô ấy đặt hàng x chục hương vị, cô ấy sẽ nhận được chính xác 12x bánh nướng nhỏ có hương vị đó. 

Yêu cầu là với mỗi hương vị, Alice phải mua đủ số lượng bánh nướng nhỏ để có ít nhất số lượng bánh nướng có hương vị đó bằng số lượng khách thích loại bánh đó. Kết quả đầu ra không phải là số lượng bánh nướng nhỏ mà là số lượng hàng chục chiếc bánh cho mỗi hương vị, cho tất cả M hương vị. 

Sự chuyển đổi quan trọng là vấn đề giảm xuống việc đếm nhu cầu cho mỗi hương vị và sau đó làm tròn nhu cầu đó lên bội số gần nhất của 12 một cách độc lập cho từng hương vị. 

Các ràng buộc đủ nhỏ để đếm tần số trực tiếp là đủ. Với N tối đa 10000 và M tối đa 100, chúng tôi có thể quét tất cả khách một cách an toàn một lần và tính toán tần suất theo thời gian tuyến tính. Ngay cả một cách tiếp cận đắt tiền hơn một chút cũng sẽ được chấp nhận, nhưng bất cứ điều gì liên quan đến việc lặp lại lồng nhau đối với khách theo mỗi hương vị sẽ là quá mức cần thiết. 

Một trường hợp thất bại tinh vi xuất hiện khi số lượng khách thưởng thức một hương vị chia hết cho 12 chứ không chia hết. Ví dụ 12 khách muốn hương 1 thì đúng 1 chục là đủ. Nếu 13 khách muốn hương 1 thì 1 chục là không đủ và chúng tôi phải mua 2 chục, tặng 24 chiếc bánh cupcake, dù chỉ cần 13 chiếc. Một cách tiếp cận đơn giản là chia bằng phép chia số nguyên mà không làm tròn số sẽ bị tính thiếu trong những trường hợp này. 

Một trường hợp khó khăn khác là khi không có vị khách nào muốn có một hương vị nào cả. Đầu ra đúng là 0 chục cho hương vị đó. Bất kỳ triển khai nào khởi tạo số đếm không chính xác hoặc quên xử lý các hương vị tần số bằng 0 đều có thể vô tình xuất ra 1 do logic làm tròn được áp dụng một cách mù quáng. 

## Phương pháp tiếp cận 

Một cách mạnh mẽ để nghĩ về điều này là mô phỏng việc mua bánh nướng nhỏ dần dần. Đối với mỗi hương vị, chúng ta có thể liên tục tăng số lượng hàng chục cho đến khi gấp 12 lần giá trị đó ít nhất là số lượng khách yêu cầu. Điều này đúng vì nó trực tiếp thực thi ràng buộc, nhưng trong trường hợp xấu nhất, chúng ta có thể lặp lại tới N/12 bước cho mỗi phiên bản. Với M hương vị, điều này trở thành O(M * N), điều này vẫn được chấp nhận ở đây nhưng gián tiếp không cần thiết. 

Quan sát trực tiếp hơn là mỗi hương vị đều độc lập. Không có sự kết hợp giữa các hương vị vì bánh nướng nhỏ có hương vị đặc trưng và các giới hạn tùy theo từng hương vị. Điều này có nghĩa là chúng ta chỉ cần số lượng tần số của từng hương vị, sau đó chuyển đổi mỗi số lượng thành mức chia trần cho 12. Sự đơn giản hóa chính là nhận ra rằng ràng buộc về bánh mì chỉ đưa ra cách làm tròn chứ không phải tương tác. 

Một khi chúng ta chấp nhận rằng mỗi hương vị rút gọn thành một phép toán số học duy nhất, thì bài toán sẽ trở thành một nhiệm vụ đếm đơn giản, sau đó là phép chia trần số nguyên. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(M * N / 12) | O(M) | Cấu trúc quá chậm, không cần thiết | 
| Tối ưu | O(N + M) | O(M) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Các bước 

1. Đọc N và M, sau đó đọc danh sách sở thích của khách. 

Điều này đưa ra sự thể hiện trực tiếp về nhu cầu cho mỗi hương vị, đây là thông tin duy nhất liên quan đến đầu ra. 
2. Khởi tạo một mảng`cnt`có kích thước M+1 về 0. 

Mỗi chỉ số tương ứng với một hương vị và chúng tôi tích lũy số lượng khách yêu cầu. 
3. Lặp lại tất cả các khách và tăng dần`cnt[f_i]`cho từng sở thích. 

Điều này nén toàn bộ đầu vào thành số lượng nhu cầu cho mỗi hương vị. 
4. Với mỗi hương vị i từ 1 đến M, tính số chục là`(cnt[i] + 11) // 12`. 

Đây là số nguyên chia trần cho 12, đảm bảo chúng ta không bao giờ mua thiếu bánh nướng nhỏ. 
5. Xuất mỗi giá trị được tính toán trên một dòng riêng. 

### Tại sao nó hoạt động 

Mỗi hương vị hoàn toàn độc lập cả về cung và cầu. Hạn chế duy nhất là phải có 12 chiếc bánh nướng nhỏ cho mỗi lần mua. Đối với bất kỳ nhu cầu số nguyên không âm d nào, số nguyên x nhỏ nhất sao cho 12x ≥ d chính xác là mức trần của d/12. Vì không có hương vị nào có thể thay thế cho hương vị khác nên việc giải quyết từng hương vị riêng biệt sẽ tạo ra giải pháp tối ưu toàn cục với tổng số bánh nướng nhỏ được mua là tối thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    arr = list(map(int, input().split()))

    cnt = [0] * (m + 1)

    for f in arr:
        cnt[f] += 1

    out = []
    for i in range(1, m + 1):
        out.append(str((cnt[i] + 11) // 12))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Việc triển khai trước tiên sẽ xây dựng một bảng tần số để tất cả các tính toán sau này là O(1) cho mỗi hương vị. Việc sử dụng`(cnt[i] + 11) // 12`là thủ thuật số nguyên tiêu chuẩn để chia trần mà không cần phép tính dấu phẩy động. 

Một điểm tinh tế là lập chỉ mục: các hương vị bắt đầu từ 1, vì vậy mảng tần số phải có kích thước M+1, không phải M. Điều này tránh được các lỗi sai sót khi khách chọn hương vị M. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3 2
1 2 1
```| Bước | cnt[1] | cnt[2] | Đầu ra calc | 
| --- | --- | --- | --- | 
| Sau khi đếm | 2 | 1 | - | 
| Cuối cùng | 2 | 1 | (2+11)//12 = 0, (1+11)//12 = 1 | 

Đầu ra:```
1
1
```Dấu vết cho thấy rằng ngay cả số lượng nhỏ dưới 12 vẫn tạo ra 1 tá vì cần ít nhất một chiếc bánh nướng nhỏ và việc mua hàng có khối 12 chiếc. 

### Mẫu 2 

đầu vào:```
14 2
1 1 1 1 1 1 1 1 1 1 1 1 1 1
```| Bước | cnt[1] | cnt[2] | Đầu ra calc | 
| --- | --- | --- | --- | 
| Sau khi đếm | 14 | 0 | - | 
| Cuối cùng | 14 | 0 | (14+11)//12 = 2, 0 | 

Đầu ra:```
2
0
```Điều này xác nhận hành vi làm tròn: 14 yêu cầu 2 chục vì một tá (12) là không đủ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N + M) | Một lượt để đếm tần số và một lượt để tính kết quả | 
| Không gian | O(M) | Mảng tần số cho hương vị M | 

Các ràng buộc cho phép tối đa 10000 khách và 100 hương vị, vì vậy giải pháp này chạy trong thời gian và bộ nhớ không đáng kể, chỉ bị chi phối bởi phân tích cú pháp đầu vào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    import io as sysio

    out = sysio.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# sample 1
assert run("3 2\n1 2 1\n") == "1\n1"

# sample 2
assert run("14 2\n" + "1 "*14 + "\n") == "2\n0"

# single guest
assert run("1 3\n2\n") == "0\n1\n0"

# all same flavor exactly 12
assert run("12 1\n" + "1 "*12 + "\n") == "1"

# boundary just over
assert run("13 1\n" + "1 "*13 + "\n") == "2"

# no demand except last flavor
assert run("5 4\n1 1 1 1 1\n") == "1\n0\n0\n0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| khách độc thân | 0 1 0 | xử lý nhu cầu thưa thớt | 
| 12 chính xác | 1 | trường hợp chia chính xác | 
| 13 hương vị duy nhất | 2 | ranh giới trần | 
| không có nhu cầu về hầu hết các hương vị | số không | xử lý số 0 đúng | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi một hương vị không có nhu cầu. Ví dụ: 

đầu vào:```
5 3
1 1 1 1 1
```Mảng đếm trở thành cnt[1]=5, cnt[2]=0, cnt[3]=0. Áp dụng phép chia trần cho 1, 0, 0. 

Thuật toán tạo ra số 0 một cách chính xác cho hương vị 2 và 3 vì`(0 + 11) // 12 = 0`. Việc triển khai thiếu sót khi thực thi nhầm tối thiểu 1 tá cho mỗi hương vị sẽ tạo ra các giá trị dương không chính xác ngay cả khi không có khách nào yêu cầu hương vị đó. 

Một trường hợp cạnh khác là bội số chính xác của 12. Với cnt[i]=12, ta có`(12+11)//12 = 1`, phù hợp với thực tế là một tá là hoàn toàn đủ. Điều này xác nhận rằng logic làm tròn không mua quá mức trong các trường hợp chia rõ ràng.
