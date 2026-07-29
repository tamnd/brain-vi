---
title: "CF 102800L - Đồ bơi"
description: "Mỗi người bơi di chuyển tới lui trên một làn đường dài m. Mỗi người bơi bắt đầu ở vị trí 0, bơi về phía vị trí m, ngay lập tức quay lại khi đến vị trí đó và lặp lại chuyển động này mãi mãi. Vận tốc của người bơi thứ i là cố định và bằng x[i] mét trên giây."
date: "2026-07-27T17:44:05+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102800
codeforces_index: "L"
codeforces_contest_name: "The 14th Jilin Provincial Collegiate Programming Contest"
rating: 0
weight: 102800
solve_time_s: 57
verified: true
draft: false
---

[CF 102800L - Đồ bơi](https://codeforces.com/problemset/problem/102800/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 57s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Mỗi người bơi di chuyển qua lại trên một làn đường dài`m`. Mỗi người bơi đều xuất phát ở vị trí`0`, bơi về phía vị trí`m`, ngay lập tức quay lại khi chạm tới nó và lặp lại chuyển động này mãi mãi. Tốc độ của người bơi`i`là cố định và bằng`x[i]`mét trên giây. Mỗi truy vấn đều yêu cầu vị trí của một vận động viên bơi lội cụ thể sau một số giây nhất định. 

Những người bơi không bao giờ tương tác với nhau nên mọi truy vấn đều hoàn toàn độc lập. Thông tin duy nhất cần thiết cho truy vấn là tốc độ của người bơi, độ dài làn đường và thời gian đã trôi qua. 

Những hạn chế là thách thức thực sự. Có thể có tới`10^6`người bơi lội và`10^6`truy vấn. Bất kỳ thuật toán nào mô phỏng chuyển động từng giây đều không thể thực hiện được ngay lập tức vì một truy vấn có thể liên quan đến tối đa`10^9`giây. Ngay cả việc mô phỏng mỗi lượt cũng sẽ thất bại, vì một người bơi nhanh có thể hoàn thành hàng trăm triệu chuyến đi. Với một triệu truy vấn, chỉ có thời gian làm việc liên tục cho mỗi truy vấn là thực tế. Chỉ riêng việc đọc đầu vào đã mất thời gian tuyến tính theo kích thước đầu vào, vì vậy thuật toán chỉ nên dành`O(1)`công việc bổ sung cho mỗi truy vấn. 

Một số trường hợp cạnh rất dễ xử lý sai. 

Hãy cân nhắc việc tiếp cận bức tường một cách chính xác.```
Input
1 5 1
1
5 1
```Người bơi đã đi chính xác`5`mét, vậy câu trả lời đúng là```
5
```Việc thực hiện bất cẩn luôn phản ánh sau khi lấy phần còn lại có thể trả về sai`0`. 

Bây giờ hãy xem xét việc hạ cánh chính xác vào cuối một chu kỳ qua lại hoàn chỉnh.```
Input
1 5 1
1
10 1
```Người bơi lội đã đi từ`0`ĐẾN`5`và quay lại`0`, vậy đáp án đúng là```
0
```Nhầm lẫn độ dài chu kỳ với`m`thay vì`2m`sẽ quay lại không chính xác`5`. 

Một sai lầm phổ biến khác là quên rằng người bơi sẽ đảo hướng sau khi chạm tới bức tường phía xa.```
Input
1 3 1
2
2 1
```Người bơi lội đi du lịch`4`tổng số mét. Sau khi đạt vị trí`3`, vẫn còn một mét chuyển động khi quay trở lại, vì vậy câu trả lời là```
2
```Đơn giản chỉ là tính toán`(speed × time) % m`sẽ quay lại không chính xác`1`. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp nhất là mô phỏng chuyển động của người bơi. Bắt đầu từ vị trí`0`, liên tục tiến người bơi cho đến khi chạm vào một trong các bức tường, đảo ngược hướng và tiếp tục cho đến khi hết thời gian yêu cầu. Điều này khớp chính xác với quá trình vật lý, vì vậy nó rõ ràng là chính xác. Thật không may, nó là quá chậm. Một truy vấn có thể kéo dài hàng tỷ giây hoặc hàng trăm triệu lượt, khiến trường hợp xấu nhất hoàn toàn không thể thực hiện được. 

Quan sát quan trọng là chuyển động của người bơi là hoàn toàn tuần hoàn. Hãy tưởng tượng kéo dài làn đường thành một đường thẳng vô tận thay vì phản chiếu vào các bức tường. Sau khi đi hết quãng đường```
distance = speed × time
```người bơi chỉ đơn giản là ở khoảng cách đó kể từ khi bắt đầu. 

Sự phản chiếu có thể được mô hình hóa bằng cách gấp đường vô hạn này mỗi lần`2m`mét. Một giai đoạn hoàn chỉnh bao gồm bơi từ`0`ĐẾN`m`và sau đó trở lại từ`m`ĐẾN`0`, cho độ dài chu kỳ là`2m`. 

Cho phép```
r = (speed × time) mod (2m)
```Nếu như`r ≤ m`, người bơi vẫn đang trong hành trình hướng ra ngoài, nên vị trí chỉ đơn giản là`r`. 

Nếu không, người bơi sẽ quay về. Khoảng cách còn lại từ bức tường phía xa là`r - m`, vậy vị trí là```
2m - r
```Mỗi truy vấn bây giờ chỉ yêu cầu một phép nhân, một phép toán modulo và một phép so sánh. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Không thể ràng buộc hiệu quả, lên đến`O(speed × time)`mô phỏng |`O(1)`| Quá chậm | 
| Tối ưu |`O(1)`mỗi truy vấn |`O(n)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số người bơi, chiều dài làn đường và số lượng truy vấn. 
2. Đọc tốc độ của tất cả người bơi thành một mảng để mỗi truy vấn có thể truy cập tốc độ của người bơi trong thời gian không đổi. 
3. Tính độ dài chu kỳ là`cycle = 2 × m`, bởi vì mỗi chuyến đi và về hoàn chỉnh đều bao gồm chính xác`2m`mét. 
4. Đối với mỗi truy vấn, truy xuất tốc độ của người bơi và tính tổng quãng đường đã đi như sau:`speed × time`. 
5. Giảm khoảng cách này theo modulo`cycle`. Mọi thứ trước chu kỳ chưa hoàn thành cuối cùng đều lặp lại chính xác, vì vậy chỉ phần còn lại mới ảnh hưởng đến vị trí hiện tại. 
6. Nếu số dư nhiều nhất là`m`, vận động viên bơi lội đang di chuyển xa khỏi vị trí ban đầu, do đó hãy xuất phần còn lại. 
7. Nếu không, người bơi sẽ di chuyển về phía điểm xuất phát. Phản chiếu phần còn lại qua bức tường phía xa bằng cách xuất ra`cycle - remainder`. 

### Tại sao nó hoạt động 

Vị trí của người bơi chỉ phụ thuộc vào vị trí của họ trong dòng chảy`2m`chu kỳ mét. Mọi chu trình đầy đủ đều kết thúc ở cùng vị trí và hướng giống như khi nó bắt đầu, do đó việc loại bỏ các chu trình hoàn chỉnh bằng phép toán modulo sẽ bảo toàn được câu trả lời. Trong một chu trình, đường đi là một đoạn thẳng từ`0`ĐẾN`m`theo sau là một đoạn thẳng từ`m`quay lại`0`. Hai công thức tương ứng chính xác với hai nửa này, do đó mọi phần còn lại có thể ánh xạ tới vị trí vật lý chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m, q = map(int, input().split())
    speeds = list(map(int, input().split()))

    cycle = 2 * m
    out = []

    for _ in range(q):
        p, k = map(int, input().split())
        r = (speeds[k - 1] * p) % cycle
        if r <= m:
            out.append(str(r))
        else:
            out.append(str(cycle - r))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Trước tiên, chương trình sẽ lưu trữ tốc độ của mỗi người bơi để mỗi truy vấn có thể truy cập tốc độ đó bằng cách lập chỉ mục trực tiếp. Vì người bơi được đánh số từ một trong khi danh sách Python được lập chỉ mục bằng 0 nên mã sử dụng`k - 1`. 

Độ dài chu kỳ được tính một lần vì nó không bao giờ thay đổi. Mọi truy vấn đều thực hiện cùng một chuỗi các phép tính số học. Số nguyên Python xử lý an toàn các giá trị lớn như`10^9 × 10^9 = 10^18`, vì vậy tràn không phải là một vấn đề. 

Việc so sánh sử dụng`<= m`còn hơn là`< m`. Khi phần còn lại chính xác là`m`, người bơi vừa tới bức tường phía xa và vẫn ở vị trí`m`. Việc sử dụng phép so sánh chặt chẽ sẽ phản ánh không chính xác điểm này chỉ bằng sự trùng hợp ngẫu nhiên và việc sử dụng các công thức khác nhau trong các ngôn ngữ khác có thể gây ra các lỗi tinh vi. 

Modulo được thực hiện với`2m`, không`m`. sử dụng`m`sẽ mất hoàn toàn thông tin về việc người bơi đang di chuyển ra ngoài hay quay trở lại. 

## Ví dụ đã hoạt động 

### Ví dụ 1```
Input
1 3 2
5
1 1
7 1
```| Truy vấn | Tốc độ | Thời gian | Tổng khoảng cách | Phần còn lại mod 6 | Trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 5 | 1 | 5 | 5 | 1 | 
| 2 | 5 | 7 | 35 | 5 | 1 | 

Cả hai truy vấn đều kết thúc ở cùng một phần còn lại vì người bơi hoàn thành một số chu kỳ đầy đủ trước khi đạt đến điểm tương tự một lần nữa. Dấu vết xác nhận rằng chỉ phần còn lại trong một chu kỳ mới quan trọng. 

### Ví dụ 2```
Input
2 5 3
2 3
2 1
2 2
5 2
```| Truy vấn | Tốc độ | Thời gian | Tổng khoảng cách | Còn lại mod 10 | Trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 2 | 2 | 4 | 4 | 4 | 
| 2 | 3 | 2 | 6 | 6 | 4 | 
| 3 | 3 | 5 | 15 | 5 | 5 | 

Truy vấn thứ hai thể hiện bước phản ánh vì phần còn lại vượt quá`m`. Truy vấn thứ ba đến chính xác ở bước ngoặt, xác nhận rằng giá trị biên được xử lý chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(n + q)`| Việc đọc tốc độ mất thời gian tuyến tính, mỗi truy vấn là thời gian không đổi. | 
| Không gian |`O(n)`| Mảng tốc độ được lưu trữ một lần. | 

Bản thân đầu vào đã chứa`n`tốc độ và`q`các truy vấn, do đó việc tiền xử lý tuyến tính là không thể tránh khỏi. Công việc liên tục cho mọi truy vấn dễ dàng thỏa mãn các ràng buộc, ngay cả khi cả hai`n`Và`q`là một triệu. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve():
    input = sys.stdin.readline
    n, m, q = map(int, input().split())
    speeds = list(map(int, input().split()))
    cycle = 2 * m
    ans = []
    for _ in range(q):
        p, k = map(int, input().split())
        r = (speeds[k - 1] * p) % cycle
        if r <= m:
            ans.append(str(r))
        else:
            ans.append(str(cycle - r))
    sys.stdout.write("\n".join(ans))

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solve()
    out = sys.stdout.getvalue()
    sys.stdin = backup_stdin
    sys.stdout = backup_stdout
    return out

# provided sample
assert run("1 3 2\n5\n1 1\n7 1\n") == "1\n1"

# minimum input
assert run("1 1 1\n1\n0 1\n") == "0"

# exact turning point
assert run("1 5 1\n1\n5 1\n") == "5"

# complete cycle
assert run("1 5 1\n1\n10 1\n") == "0"

# reflection
assert run("1 3 1\n2\n2 1\n") == "2"

# multiple swimmers
assert run("2 5 2\n2 3\n2 1\n2 2\n") == "4\n4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Người bơi đơn, không có thời gian |`0`| Đầu vào tối thiểu và không có chuyển động | 
| Vị trí tiếp cận`m`chính xác |`5`| Xử lý đúng bước ngoặt | 
| Chu kỳ hoàn chỉnh |`0`| Đúng thời gian modulo của`2m`| 
| Ví dụ phản ánh |`2`| Trả lại một nửa chu kỳ | 
| Nhiều người bơi |`4`,`4`| Lập chỉ mục chính xác vào mảng tốc độ | 

## Vỏ cạnh 

Khi vận động viên bơi tới đúng bức tường phía xa, phần còn lại bằng`m`.```
Input
1 5 1
1
5 1
```Thuật toán tính toán`r = 5 mod 10 = 5`. Từ`5 ≤ 5`, nó trả về`5`, phù hợp với vị trí thực tế của người bơi trên tường. 

Khi người bơi hoàn thành toàn bộ hành trình khứ hồi, phần còn lại sẽ bằng không.```
Input
1 5 1
1
10 1
```Thuật toán tính toán`r = 10 mod 10 = 0`. Nhánh đầu tiên trở lại`0`, chính xác là vị trí bắt đầu sau một chu kỳ hoàn chỉnh. 

Khi người bơi đã quay lại, cần phải phản xạ.```
Input
1 3 1
2
2 1
```Tổng khoảng cách là`4`. Thuật toán tính toán`r = 4 mod 6 = 4`. Từ`4 > 3`, nó trả về`6 - 4 = 2`. Về mặt thể chất, người bơi đạt đến vị trí`3`sau đó`1.5`giây rồi bơi lùi thêm một mét nữa, kết thúc ở vị trí`2`, khớp chính xác với kết quả tính toán.
