---
title: "CF 102800E – Rút gọn mảng"
description: "Chúng tôi liên tục rút ngắn một mảng bằng cách chọn hai giá trị dương liền kề. Hai giá trị đó sẽ bị xóa và thay thế bằng a % b hoặc b % a, được chọn tự do cho thao tác đó. Vì hai phần tử trở thành một, nên mọi thao tác đều giảm độ dài mảng đi đúng một."
date: "2026-07-27T17:37:19+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102800
codeforces_index: "E"
codeforces_contest_name: "The 14th Jilin Provincial Collegiate Programming Contest"
rating: 0
weight: 102800
solve_time_s: 51
verified: true
draft: false
---

[CF 102800E - Rút ngắn mảng](https://codeforces.com/problemset/problem/102800/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 51s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi liên tục rút ngắn một mảng bằng cách chọn hai giá trị dương liền kề. Hai giá trị đó bị xóa và thay thế bằng một trong hai`a % b`hoặc`b % a`, được chọn tự do cho hoạt động đó. Vì hai phần tử trở thành một, nên mọi thao tác đều giảm độ dài mảng đi đúng một. 

Câu hỏi không phải là giá trị cuối cùng trở thành gì mà là có thể loại bỏ bao nhiêu phần tử. Khi không tồn tại cặp số dương liền kề thì không thể thực hiện thêm thao tác nào nữa. Chúng ta phải tính toán độ dài cuối cùng tối thiểu có thể đạt được. 

Độ dài mảng có thể lớn bằng`10^6`trong một trường hợp thử nghiệm duy nhất, với tối đa`10`trường hợp thử nghiệm. Bất kỳ thuật toán nào khám phá các thứ tự hoạt động khác nhau hoặc mô phỏng nhiều khả năng sẽ bị loại trừ ngay lập tức. Ngay cả thời gian bậc hai cũng cần khoảng`10^12`hoạt động trên đầu vào lớn nhất, điều này hoàn toàn không khả thi. Giải pháp phải xử lý từng mảng theo thời gian tuyến tính. 

Phần khó khăn là chúng ta được phép chọn một trong hai phần còn lại. Sự tự do này có nghĩa là các giá trị thực tế ít quan trọng hơn nhiều so với những gì chúng xuất hiện lần đầu. 

Hãy xem xét mảng`[1, 1, 1]`. Chúng ta có thể thay thế hai phần tử đầu tiên bằng`1 % 1 = 0`, thu được`[0, 1]`. Vì cặp còn lại không cùng dương nên quá trình dừng lại khi có độ dài`2`. Một nỗ lực tham lam để luôn hợp nhất bất cứ khi nào có thể không thể đạt được độ dài`1`. 

Bây giờ hãy xem xét`[2, 3]`. Từ`2 % 3 = 2`Và`3 % 2 = 1`, không lựa chọn nào tạo ra số không. Mảng luôn trở thành một giá trị dương duy nhất, vì vậy câu trả lời là`1`. 

Một trường hợp quan trọng khác là`[5, 5, 5, 5]`. Các giá trị bằng nhau cho phép tạo ra số 0 ngay lập tức vì`5 % 5 = 0`. Lặp lại ý tưởng này để lại hai số 0, vì vậy độ dài tối thiểu là`2`, không`1`. Việc coi mọi khối dương là có thể rút gọn hoàn toàn sẽ đưa ra câu trả lời sai. 

## Phương pháp tiếp cận 

Giải pháp brute-force sẽ khám phá mọi phép toán hợp lệ, mọi cặp liền kề có thể có và cả hai lựa chọn còn lại. Vì mỗi hoạt động đều thay đổi các khả năng trong tương lai nên việc tìm kiếm sẽ phân nhánh theo cấp số nhân. Ngay cả việc ghi nhớ cũng không hiệu quả vì số lượng mảng có thể truy cập là rất lớn. Cách tiếp cận này đúng vì nó xem xét mọi trình tự hợp lệ, nhưng nó trở nên vô vọng ngay cả đối với các mảng có vài chục phần tử. 

Quan sát quan trọng là các số 0 sẽ chặn vĩnh viễn các lần hợp nhất trong tương lai. Vì chỉ có thể hợp nhất các số dương liền kề nên mọi khối giá trị dương liền kề tối đa sẽ hoạt động độc lập. Các hoạt động bên trong một khối không bao giờ tương tác với khối khác vì số 0 không bao giờ có thể biến mất. 

Câu hỏi còn lại là một khối tích cực có thể giảm được bao xa. 

Giả sử mọi giá trị bên trong khối đều giống hệt nhau. Mọi sự hợp nhất đều tạo ra số 0 vì`x % x = 0`. Mỗi lần hợp nhất sẽ tạo ra một dấu phân cách, ngăn số 0 đó tham gia lại. Bắt đầu với một khối có chiều dài`k`, mỗi lần hợp nhất sẽ sử dụng chính xác hai số dương và tạo ra một số 0. Cuối cùng chỉ còn lại số 0, cộng thêm có thể là một số dương chưa được chạm tới nếu`k`thật kỳ quặc. Độ dài cuối cùng của khối đó là`ceil(k / 2)`. 

Bây giờ giả sử khối chứa ít nhất hai giá trị khác nhau. Khi đó ước chung lớn nhất của khối luôn có thể được bảo toàn trong khi áp dụng thuật toán Euclide nhiều lần. Vì các số không bằng nhau cuối cùng tạo ra số dư dương nhỏ hơn rất nhiều, nên chúng ta có thể tránh tạo các số 0 chặn cho đến khi chỉ còn lại một giá trị dương. Toàn bộ khối sụp đổ thành một phần tử duy nhất. 

Điều này hoàn toàn đặc trưng cho mọi khối tích cực. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu | O(N) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Quét mảng từ trái sang phải. 
2. Bất cứ khi nào gặp số 0, hãy bỏ qua nó vì nó đã phân tách các vùng độc lập. 
3. Khi tìm thấy giá trị dương, hãy xác định toàn bộ khối dương liền kề tối đa. 
4. Trong khi quét khối đó, hãy kiểm tra xem mọi phần tử có bằng phần tử đầu tiên hay không. Đồng thời, ghi lại chiều dài khối. 
5. Nếu mọi giá trị trong khối đều giống hệt nhau, hãy thêm`(length + 1) // 2`để trả lời. Đây chính xác là`ceil(length / 2)`. 
6. Nếu không, hãy thêm`1`cho câu trả lời vì toàn bộ khối có thể được rút gọn thành một phần tử duy nhất. 
7. Tiếp tục quét sau khi kết thúc khối cho đến khi toàn bộ mảng được xử lý. 

### Tại sao nó hoạt động 

Số 0 là rào cản vĩnh viễn vì chỉ các số dương liền kề mới có thể tham gia vào một hoạt động. Mọi hoạt động đều nằm trong một khối tích cực, vì vậy các khối khác nhau không bao giờ ảnh hưởng lẫn nhau. 

Bên trong một khối chứa ít nhất hai giá trị riêng biệt, các ứng dụng lặp đi lặp lại của thuật toán Euclide sẽ giảm toàn bộ khối xuống một giá trị dương mà không đưa ra số 0 không thể tránh khỏi. Bên trong một khối mà mọi giá trị đều giống nhau, mọi sự hợp nhất có thể ngay lập tức tạo ra số 0, chia các phần dương còn lại thành các phần độc lập nhỏ hơn. Quá trình đó để lại chính xác`ceil(length / 2)`những yếu tố còn sót lại. Vì mỗi khối là độc lập nên việc tính tổng các đóng góp này sẽ cho độ dài mảng cuối cùng tối thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    out = []

    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        ans = 0
        i = 0

        while i < n:
            if a[i] == 0:
                i += 1
                continue

            first = a[i]
            same = True
            j = i

            while j < n and a[j] > 0:
                if a[j] != first:
                    same = False
                j += 1

            length = j - i

            if same:
                ans += (length + 1) // 2
            else:
                ans += 1

            i = j

        out.append(str(ans))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Vòng lặp bên ngoài xử lý từng trường hợp kiểm thử một cách độc lập. Con trỏ`i`quét mảng một lần và mỗi khối dương được kiểm tra chính xác một lần. 

Đối với mỗi khối, mã ghi nhớ giá trị đầu tiên và kiểm tra xem mọi giá trị sau có khớp với giá trị đó hay không. Điều này tránh việc lưu trữ bất kỳ cấu trúc dữ liệu bổ sung nào trong khi xác định xem khối có đồng nhất hay không. 

biểu hiện`(length + 1) // 2`tính toán`ceil(length / 2)`sử dụng số học số nguyên. Đây là trường hợp đặc biệt duy nhất cần thiết cho khối thống nhất. 

Sau khi hoàn thành một khối, quá trình quét sẽ chuyển thẳng đến phần cuối của khối đó. Không có phần tử nào được xem lại, đó là lý do tại sao việc triển khai vẫn tuyến tính. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1
3
1 1 1
```| Bước | Khối hiện tại | Chiều dài | Tất cả đều bình đẳng | Đóng góp | Trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 1 |`[1,1,1]`| 3 | Có | 2 | 2 | 

Toàn bộ mảng là một khối dương thống nhất. Mỗi lần hợp nhất ngay lập tức tạo ra số 0, do đó chỉ còn lại hai phần tử. Ví dụ này cho thấy tại sao một khối dương không phải lúc nào cũng có thể rút gọn thành một phần tử. 

### Ví dụ 2 

đầu vào:```
1
5
2 3 4 0 7
```| Bước | Khối hiện tại | Chiều dài | Tất cả đều bình đẳng | Đóng góp | Trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 1 |`[2,3,4]`| 3 | Không | 1 | 1 | 
| 2 |`[7]`| 1 | Có | 1 | 2 | 

Số 0 chia mảng thành hai khối độc lập. Phần đầu tiên chứa các giá trị khác nhau và thu gọn thành một phần tử. Cái thứ hai đã có chiều dài một. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N) | Mỗi phần tử mảng được quét chính xác một lần. | 
| Không gian | O(1) | Chỉ có một vài biến được duy trì trong khi quét. | 

Quét tuyến tính dễ dàng đáp ứng các ràng buộc, ngay cả khi mảng chứa một triệu phần tử. Bộ nhớ phụ không đổi cũng vẫn nằm trong giới hạn bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve():
    input = sys.stdin.readline
    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        ans = 0
        i = 0
        while i < n:
            if a[i] == 0:
                i += 1
                continue
            first = a[i]
            same = True
            j = i
            while j < n and a[j] > 0:
                if a[j] != first:
                    same = False
                j += 1
            length = j - i
            ans += (length + 1) // 2 if same else 1
            i = j
        out.append(str(ans))
    return "\n".join(out)

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    print(solve(), end="")
    return sys.stdout.getvalue()

assert run("1\n3\n1 1 1\n") == "2", "sample"
assert run("1\n2\n3 4\n") == "1", "two distinct values"
assert run("1\n4\n5 5 5 5\n") == "2", "all equal"
assert run("1\n5\n2 3 4 0 7\n") == "2", "zero separates blocks"
assert run("1\n2\n9 9\n") == "1", "minimum uniform block"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`3 4`|`1`| Hai giá trị không bằng nhau sụp đổ hoàn toàn. | 
|`5 5 5 5`|`2`| Khối thống nhất yêu cầu`ceil(length / 2)`. | 
|`2 3 4 0 7`|`2`| Các số 0 chia mảng thành các vùng độc lập. | 
|`9 9`|`1`| Khối thống nhất không tầm thường nhỏ nhất. | 

## Vỏ cạnh 

Hãy xem xét đầu vào:```
1
3
1 1 1
```Thuật toán tìm một khối có độ dài dương`3`, quan sát rằng mọi giá trị đều giống hệt nhau và đóng góp`(3 + 1) // 2 = 2`. Đầu ra là`2`, phù hợp với trình tự hoạt động tối ưu. 

Bây giờ hãy xem xét:```
1
2
2 3
```Chiều dài khối là`2`, nhưng các giá trị không giống nhau. Thuật toán góp phần`1`, nhận biết chính xác rằng các số không bằng nhau luôn có thể được hợp nhất thành một giá trị còn lại. 

Cuối cùng, hãy xem xét:```
1
5
4 4 0 6 7
```Quá trình quét phát hiện hai khối độc lập. Đầu tiên là đồng phục có đóng góp`1`. Thứ hai chứa các giá trị riêng biệt với sự đóng góp`1`. Tổng của họ là`2`. Số 0 không bao giờ bị vượt qua trong quá trình xử lý, khớp chính xác với hạn chế mà các hoạt động không thể liên quan đến các lân cận không dương.
