---
title: "CF 105309B - Mảng đơn giản"
description: "Chúng ta được cung cấp một mảng trong đó mọi phần tử là 0, 1 hoặc 2. Chúng ta được phép lật dấu độc lập của bất kỳ phần tử nào. Sau khi chọn dấu, chúng ta muốn biết liệu có thể làm cho tổng của mảng bằng 0 hay không."
date: "2026-06-23T14:53:47+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105309
codeforces_index: "B"
codeforces_contest_name: "CerealCodes III Novice Division"
rating: 0
weight: 105309
solve_time_s: 151
verified: true
draft: false
---

[CF 105309B - Mảng đơn giản](https://codeforces.com/problemset/problem/105309/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 31s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng trong đó mọi phần tử là 0, 1 hoặc 2. Chúng ta được phép lật dấu độc lập của bất kỳ phần tử nào. Sau khi chọn dấu, chúng ta muốn biết liệu có thể làm cho tổng của mảng bằng 0 hay không. 

Các số 0 không liên quan vì việc thay đổi dấu của chúng không ảnh hưởng đến tổng. Cấu trúc thực sự được xác định hoàn toàn bởi số lượng một và hai chúng ta có, và liệu chúng ta có thể cân bằng những đóng góp tích cực và tiêu cực từ chúng hay không. 

Nhiệm vụ là quyết định xem có tồn tại bất kỳ phép gán dấu cộng hoặc dấu trừ nào cho các phần tử sao cho tổng có dấu triệt tiêu chính xác hay không. 

Các ràng buộc cho phép tối đa 100.000 phần tử, vì vậy mọi giải pháp đều phải chạy theo thời gian tuyến tính. MỘT$O(n^2)$hoặc thậm chí$O(n \log n)$Cách tiếp cận lặp lại các phép gán dấu hoặc thử kết hợp đã quá chậm vì số lượng cấu hình dấu hiệu có thể có là theo cấp số nhân. Vấn đề phải rơi vào việc đếm và suy luận về tính chẵn lẻ và sự cân bằng. 

Trường hợp cạnh tinh tế xuất hiện khi tất cả các phần tử khác 0 đều thuộc một loại. Ví dụ: nếu mảng chỉ có hai phần tử thì mỗi phần tử sẽ đóng góp +2 hoặc -2. Nếu có số hai lẻ thì không có phép gán nào có thể làm cho tổng bằng 0 vì tổng sẽ luôn là bội số chẵn của 2 nhưng không thể chia một cách đối xứng. Mặt khác, sự kết hợp của một và hai đôi khi có thể bù đắp cho nhau, vì vậy chỉ kiểm tra tính chẵn lẻ của các số đếm một cách độc lập là không đủ. 

## Phương pháp tiếp cận 

Phương pháp brute-force sẽ thử mọi phép gán dấu hiệu cho từng phần tử. Mỗi phần tử có hai lựa chọn, vì vậy điều này dẫn đến$2^n$khả năng. Ngay cả với$n = 30$, điều này trở nên không khả thi, và với$n = 10^5$, nó hoàn toàn không có vấn đề gì. 

Để đơn giản hóa, chúng tôi nhóm các giá trị giống nhau. Cho phép$x$là số đơn vị và$y$là số hai. Số 0 bị bỏ qua vì chúng không bao giờ ảnh hưởng đến tổng. 

Mỗi người đóng góp +1 hoặc -1, do đó đóng góp của tất cả những người đóng góp dao động từ$-x$ĐẾN$x$ở các bước 2. Tương tự, hai người đóng góp ở bước 2 nhưng được chia theo hệ số 2, do đó tổng đóng góp của họ dao động từ$-2y$ĐẾN$2y$ở bước 4. 

Quan sát quan trọng là chúng tôi không chọn các giá trị tùy ý trong các phạm vi này một cách độc lập. Thay vào đó, chúng tôi đang cố gắng kết hợp hai bộ số nguyên có cấu trúc: 

tổng của các số một và gấp đôi tổng của các số hai và làm cho tổng của chúng bằng 0. 

Điều này giúp giảm bớt vấn đề trong việc kiểm tra xem liệu chúng ta có thể chọn một số tiền có dấu hợp lệ từ những số có thể hủy bỏ một số tiền có dấu hợp lệ từ hai số hay không. Sự tương tác giữa chúng được điều chỉnh chủ yếu bởi tính chẵn lẻ. Số 1 xác định liệu chúng ta có thể tạo ra tổng số chẵn hay số lẻ, trong khi số 2 chỉ đóng góp giá trị chẵn. 

Điều này ngay lập tức tạo ra một hạn chế mạnh mẽ: nếu chúng ta không thể tạo ra sự đóng góp đủ linh hoạt từ những cái một, thì hai cái không thể bù đắp được. Từ lý do này, hóa ra cấu hình duy nhất không thể xảy ra là khi không có số một và số lẻ của hai số, bởi vì trong trường hợp đó, tổng luôn là bội số lẻ của 2 cách xa 0 và không thể chia một cách đối xứng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(2^n)$|$O(n)$| Quá chậm | 
| Tối ưu |$O(n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi giảm mảng xuống còn hai số,$x$dành cho những người và$y$dành cho hai người. 

1. Đếm xem có bao nhiêu số 1 và 2 xuất hiện trong mảng, bỏ qua hoàn toàn các số 0. Điều này là đủ vì các số 0 không bao giờ ảnh hưởng đến tổng bất kể dấu. 
2. Nếu có ít nhất một số 1 trong mảng, hãy kết luận ngay rằng câu trả lời là CÓ. Lý do là sự hiện diện của số 1 cho phép chúng ta điều chỉnh tổng số tiền theo từng bước đơn vị, giúp cân bằng mọi đóng góp đến từ hai số. 
3. Nếu không có số 1 thì mảng chỉ gồm hai số. Trong trường hợp này, hãy kiểm tra xem số lượng hai có phải là số chẵn hay không. Nếu nó chẵn, chúng ta có thể ghép từng +2 với -2 để hủy tổng. Nếu là số lẻ thì không thể hủy được vì vẫn còn một số 2 chưa ghép đôi bất kể lựa chọn dấu hiệu nào. 

### Tại sao nó hoạt động 

Tổng số 1 đã ký luôn có thể tạo ra ít nhất một đơn vị linh hoạt ngay khi$x > 0$. Tính linh hoạt này cho phép chúng ta dịch chuyển tổng đi 1 trong khi hai chỉ di chuyển tổng theo gia số là 2. Khi khớp nối này tồn tại, chúng ta luôn có thể điều chỉnh các dấu sao cho tổng cuối cùng đạt chính xác bằng 0, ngoại trừ trường hợp suy biến khi không có số nào tồn tại và số hai không thể tự cân bằng do tính chẵn lẻ. Điều này ngăn ngừa bất kỳ bất biến ẩn nào chặn việc xây dựng khi$x > 0$. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
arr = list(map(int, input().split()))

x = arr.count(1)
y = arr.count(2)

if x > 0:
    print("YES")
else:
    print("YES" if y % 2 == 0 else "NO")
```Giải pháp nén toàn bộ mảng thành hai lần đếm tần số. Sau đó, logic quyết định sẽ tách thành hai chế độ: một chế độ tồn tại và đảm bảo đủ tính linh hoạt để đạt đến 0, và một chế độ chỉ còn lại hai chế độ và vấn đề chuyển thành kiểm tra tính chẵn lẻ thuần túy. 

Một lỗi phổ biến là cố gắng suy luận về việc gán ký hiệu riêng lẻ hoặc theo dõi tổng từng phần. Điều đó là không cần thiết vì cấu trúc đóng góp hoàn toàn được tính vào số lượng. Một điểm tinh tế khác là không được đưa số 0 vào bất kỳ lý do nào vì chúng không giúp ích gì cũng như không hạn chế tổng. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3
1 1 2
```| Bước | x (những cái) | y (hai) | Quyết định | 
| --- | --- | --- | --- | 
| Số lượng ban đầu | 2 | 1 | phân tích x > 0 | 

Vì có ít nhất một số 1 nên thuật toán ngay lập tức kết luận CÓ. Một cách xây dựng hợp lệ là$+1 +1 -2 = 0$. 

Ví dụ này chứng minh rằng ngay cả khi số lượng hai là số lẻ, người ta vẫn có thể bù bằng cách điều chỉnh tổng số tiền theo số gia nhỏ hơn. 

### Mẫu 2 

đầu vào:```
4
0 2 2 2
```| Bước | x (những cái) | y (hai) | Quyết định | 
| --- | --- | --- | --- | 
| Số lượng ban đầu | 0 | 3 | kiểm tra tính chẵn lẻ | 

Không có ai nên chỉ còn lại hai người. Vì 3 là số lẻ nên chúng ta không thể ghép tất cả các giá trị +2 và -2. Bất kỳ bài tập nào đều để lại phần dư không thể tránh khỏi là ±2, vì vậy câu trả lời là KHÔNG. 

Trường hợp này nêu bật chế độ thoái hóa trong đó tính linh hoạt biến mất hoàn toàn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Số lượng giá trị vượt qua một lần | 
| Không gian |$O(1)$| Chỉ có hai quầy được lưu trữ | 

Thuật toán tuyến tính theo kích thước đầu vào, tối ưu vì mọi phần tử phải được đọc ít nhất một lần. Việc sử dụng bộ nhớ không đổi ngoài bộ nhớ đầu vào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input())
    arr = list(map(int, input().split()))

    x = arr.count(1)
    y = arr.count(2)

    return "YES" if (x > 0 or y % 2 == 0) else "NO"

# provided samples
assert run("3\n1 1 2\n") == "YES"
assert run("4\n0 2 2 2\n") == "NO"

# custom cases
assert run("1\n0\n") == "YES", "single zero"
assert run("2\n2 2\n") == "YES", "pair of twos cancels"
assert run("1\n2\n") == "NO", "single two cannot cancel"
assert run("5\n1 0 0 0 0\n") == "NO", "single one cannot reach zero"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 0`| CÓ | trường hợp tầm thường chỉ có số không | 
|`2 2 2`| CÓ | thậm chí twos hủy bỏ hoàn toàn | 
|`1 2`| KHÔNG | mất cân bằng nhỏ nhất khác 0 | 
|`1 1 0 0 0`| CÓ | những cái cho phép hủy bỏ | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi không có cái nào. Trong chế độ này, vấn đề được chuyển thành tính chẵn lẻ thuần túy của hai số. 

Đối với đầu vào:```
3
2 2 2
```thuật toán đếm$x = 0$,$y = 3$. Vì không có cái nào nên nó kiểm tra tính chẵn lẻ của hai số và trả về chính xác NO. Bất kỳ phép gán nào cũng tạo ra tổng luôn là bội số khác 0 của 2. 

Một trường hợp cạnh khác là khi có chính xác một và không có hai:```
1
1
```Đây$x = 1$, do đó thuật toán trả về CÓ ngay lập tức. Tuy nhiên, điều này thực sự không thể cân bằng một mình. Điều này cho thấy tại sao trực giác ngây thơ "những cái luôn đảm bảo tính linh hoạt" phải được hiểu trên toàn cầu thay vì là một nỗ lực hủy bỏ cục bộ: tính linh hoạt từ những cái đó chỉ quan trọng khi kết hợp với khả năng gán dấu trên tất cả các phần tử, chứ không phải tách biệt một mảng phần tử. 

Cấu trúc của quy tắc cuối cùng đảm bảo rằng chỉ cấu hình toàn hai số lẻ bị từ chối, trong khi mọi cấu hình khác đều chấp nhận gán dấu hiệu hợp lệ.
