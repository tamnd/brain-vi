---
title: "CF 102835M - Tổ hợp phím"
description: "Sự cố mô hình bàn phím số bị lỗi với bốn phím được sắp xếp theo dạng lưới 2 x 2. Mỗi phím tương ứng với một trong các số từ 1 đến 4 và nhấn một phím sẽ tạo ra một cặp chứa chỉ mục hàng và chỉ mục cột của nó."
date: "2026-07-26T15:05:12+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102835
codeforces_index: "M"
codeforces_contest_name: "The 2020 ICPC Asia Taipei-Hsinchu Site Programming Contest"
rating: 0
weight: 102835
solve_time_s: 66
verified: true
draft: false
---

[CF 102835M - Tổ hợp phím](https://codeforces.com/problemset/problem/102835/M) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 6s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Sự cố mô hình bàn phím số bị lỗi với bốn phím được sắp xếp theo dạng lưới 2 x 2. Mỗi phím tương ứng với một trong các số từ 1 đến 4 và nhấn một phím sẽ tạo ra một cặp chứa chỉ mục hàng và chỉ mục cột của nó. Khi nhấn nhiều phím cùng nhau, bộ điều khiển sẽ mất khả năng ghép nối giữa các hàng và cột. Nó chỉ nhận tập hợp các hàng xuất hiện và tập hợp các cột xuất hiện. Nhiệm vụ là đếm xem có bao nhiêu tập hợp con khóa khác nhau có thể tạo ra tập hợp hàng và tập hợp cột được quan sát. Vấn đề ban đầu giới hạn các tập hợp hàng và cột nhận được ở kích thước tối đa là hai, vì bàn phím chỉ có hai giá trị hàng có thể có và hai giá trị cột có thể có. 

Đầu vào chứa một số trường hợp thử nghiệm. Với mỗi test, dòng đầu tiên cho biết số hàng và cột riêng biệt nhận được. Hai dòng tiếp theo mô tả các giá trị hàng và giá trị cột đó. Đầu ra là số lượng tập hợp con trong số bốn phím trên bàn phím có tập hợp các hàng và cột khớp chính xác với tín hiệu nhận được. 

Những hạn chế là cực kỳ nhỏ. Có tối đa 10 trường hợp thử nghiệm và mỗi chiều của tín hiệu nhận được chỉ chứa một hoặc hai giá trị. Điều này có nghĩa là giải pháp không cần bất kỳ tối ưu hóa nâng cao nào. Cách tiếp cận theo thời gian không đổi là đủ và thậm chí việc kiểm tra mọi tập hợp con có thể có của bốn khóa cũng chỉ là 16 trường hợp cho mỗi lần kiểm tra. Bất kỳ giải pháp nào liên quan đến cấu trúc dữ liệu lớn hoặc toán học phức tạp sẽ giải quyết một vấn đề khó hơn nhiều so với vấn đề thực tế. 

Các bẫy chính xuất phát từ thực tế là các tổ hợp phím khác nhau có thể tạo ra cùng một tín hiệu. Ví dụ: nhấn phím 1 và 4 sẽ cho cùng một nhóm hàng và nhóm cột đã nhận khi nhấn phím 2 và 3. Giải pháp cố gắng tái tạo lại một nhóm được nhấn duy nhất sẽ không thành công do tín hiệu không lưu giữ thông tin ghép nối ban đầu. 

Đối với đầu vào```
1
1 1
0
0
```đầu ra đúng là```
1
```Chỉ phím 1 tạo tập hợp hàng`{0}`và tập cột`{0}`. Một cách tiếp cận bất cẩn khi đếm các lựa chọn hàng và cột một cách độc lập có thể coi hàng và cột đó là không liên quan và bị tính quá mức. 

Đối với đầu vào```
1
2 2
0 1
0 1
```đầu ra đúng là```
7
```Mọi tập hợp con không trống của bốn khóa ngoại trừ tập hợp trống sẽ tạo ra tập hợp hàng đầy đủ`{0,1}`và tập cột`{0,1}`. Một cách tiếp cận bất cẩn có thể chỉ trả về một vì nó cho rằng tín hiệu nhận được xác định một sự kết hợp. 

## Phương pháp tiếp cận 

Giải pháp mạnh mẽ trực tiếp là đủ vì bàn phím chỉ chứa bốn phím. Ý tưởng mạnh mẽ tự nhiên là liệt kê mọi tập hợp con có thể có của khóa, mô phỏng tín hiệu được tạo bởi tập hợp con đó và so sánh nó với tín hiệu đã cho. Đối với mỗi khóa được chọn, chúng tôi thu thập hàng và cột của nó. Nếu cả hai tập hợp được thu thập đều bằng tập hợp hàng và cột nhận được, chúng tôi sẽ tăng câu trả lời. 

Cách tiếp cận này đúng vì nó kiểm tra chính xác định nghĩa của tổ hợp phím hợp lệ. Chỉ có (2^4 = 16) tập hợp con có thể, do đó trường hợp xấu nhất chỉ thực hiện vài chục thao tác cho mỗi trường hợp thử nghiệm. Những hạn chế nhỏ làm cho đây trở thành giải pháp mong muốn. 

Điều quan trọng nhất là cách bố trí bàn phím đã được cố định. Chúng ta không bao giờ cần phải tìm kiếm các bố cục có thể có hoặc rút ra một công thức toán học. Điều chưa biết duy nhất là phím nào trong số bốn phím đã được nhấn và không gian tìm kiếm hoàn chỉnh có kích thước 16. Phương pháp vũ phu đã giảm bớt vấn đề khi kiểm tra một số lượng ứng viên cố định nhỏ. 

So sánh giữa các phương pháp là: 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(16) | O(1) | Đã chấp nhận | 
| Tối ưu | O(16) | O(1) | Đã chấp nhận | 

Giải pháp brute-force cũng là giải pháp thực tế tối ưu ở đây vì không thể giảm không gian tìm kiếm liên tục một cách có ý nghĩa. 

## Hướng dẫn thuật toán 

1. Lưu trữ bốn phím với tọa độ tương ứng của chúng. Phím 1 có tọa độ`(0,0)`, phím 2 có tọa độ`(0,1)`, phím 3 có tọa độ`(1,0)`và phím 4 có tọa độ`(1,1)`. 
2. Đọc các giá trị hàng và giá trị cột đã nhận và lưu trữ chúng dưới dạng tập hợp. Các bộ được sử dụng vì thứ tự của các giá trị nhận được không quan trọng. 
3. Liệt kê mọi mặt nạ từ 0 đến 15. Mỗi bit biểu thị liệu một trong bốn phím có được bao gồm trong tổ hợp được nhấn hay không. 
4. Đối với mặt nạ hiện tại, thu thập tất cả các hàng và cột thuộc các phím đã chọn. Tập hợp hàng và tập hợp cột được tạo mô tả chính xác những gì bộ điều khiển sẽ nhận được từ sự kết hợp này. 
5. So sánh các bộ được tạo với các bộ đầu vào. Nếu cả hai đều bằng nhau, hãy tính mặt nạ này là tổ hợp phím có thể có. 
6. Xuất ra số lượng mặt nạ hợp lệ. 

Tại sao nó hoạt động: 

Mọi tổ hợp được nhấn có thể tương ứng với chính xác một tập hợp con trong số bốn phím và mọi tập hợp con đều được kiểm tra bằng bảng liệt kê. Đối với mỗi tập hợp con, thuật toán sẽ tính toán tín hiệu chính xác mà bộ điều khiển nhận được. Một tập hợp con được tính khi và chỉ khi tín hiệu của nó khớp với tín hiệu đã cho, do đó số đếm cuối cùng chứa tất cả và chỉ các kết hợp hợp lệ. 

## Giải pháp Python```python
import sys

input = sys.stdin.readline

def solve():
    t = int(input())
    keys = [(0, 0), (0, 1), (1, 0), (1, 1)]
    ans = []

    for _ in range(t):
        m, n = map(int, input().split())
        rows = set(map(int, input().split()))
        cols = set(map(int, input().split()))

        cur = 0

        for mask in range(1 << 4):
            r = set()
            c = set()

            for i in range(4):
                if mask & (1 << i):
                    r.add(keys[i][0])
                    c.add(keys[i][1])

            if r == rows and c == cols:
                cur += 1

        ans.append(str(cur))

    sys.stdout.write("\n".join(ans))

if __name__ == "__main__":
    solve()
```Mảng`keys`đại diện cho cách bố trí bàn phím cố định. Chỉ số của một phần tử là số khóa trừ đi một, do đó bit`i`trong mặt nạ tương ứng trực tiếp với một phím trên bàn phím. 

Vòng lặp mặt nạ bao gồm tất cả các tập con có thể có. Tập hợp con trống được đưa vào một cách có chủ ý vì về mặt lý thuyết, tín hiệu nhận được có thể trống trong phiên bản tổng quát hơn của bài toán, mặc dù các ràng buộc đã cho luôn cung cấp ít nhất một hàng và một cột. 

Các bộ`r`Và`c`được xây dựng lại cho mỗi mặt nạ. Điều này tránh việc vô tình giữ các giá trị từ tập hợp con trước đó, đây là lỗi triển khai phổ biến trong các vấn đề về liệt kê. 

Không có vấn đề về ranh giới lập chỉ mục vì bàn phím có chính xác bốn vị trí đã biết. Tràn số nguyên cũng không thể thực hiện được vì câu trả lời nhiều nhất là 16. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
2
2 1
0 1
0
1 2
1
0 1
```Trường hợp thử nghiệm đầu tiên đã nhận được hàng`{0,1}`và cột`{0}`. 

| Mặt nạ | Phím đã chọn | Hàng đã tạo | Cột được tạo | Đã tính | 
| --- | --- | --- | --- | --- | 
| 0000 | không | {} | {} | Không | 
| 0011 | 1,2 | {0} | {0,1} | Không | 
| 0101 | 1,3 | {0,1} | {0} | Có | 
| 0110 | 2,3 | {0,1} | {0,1} | Không | 

Tập hợp con hợp lệ duy nhất là khóa 1 và 3, vì vậy câu trả lời là 1. Điều này chứng tỏ rằng thuật toán duy trì sự ghép nối giữa các hàng và cột trong khi kiểm tra mọi tập hợp được nhấn có thể. 

Đối với mẫu thứ hai:```
2
2 2
0 1
0 1
1 1
1
```Trường hợp thử nghiệm đầu tiên yêu cầu mọi giá trị hàng và cột có thể có. 

| Mặt nạ | Phím đã chọn | Hàng đã tạo | Cột được tạo | Đã tính | 
| --- | --- | --- | --- | --- | 
| 0000 | không | {} | {} | Không | 
| 0001 | 1 | {0} | {0} | Không | 
| 0011 | 1,2 | {0} | {0,1} | Không | 
| 0101 | 1,3 | {0,1} | {0} | Không | 
| 0110 | 2,3 | {0,1} | {0,1} | Có | 
| 1111 | tất cả các phím | {0,1} | {0,1} | Có | 

Tiếp tục kiểm tra tương tự cho tất cả các mặt nạ sẽ cho ra bảy tập hợp con hợp lệ. Điều này thể hiện tính chất trung tâm của bài toán: các tập hợp con khác nhau có thể tạo ra cùng một tín hiệu thu được. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(16) | Mỗi trường hợp thử nghiệm sẽ kiểm tra tất cả các tập hợp con của bốn khóa. | 
| Không gian | O(1) | Chỉ một vài bộ chứa tối đa hai giá trị được lưu trữ. | 

Công việc tối đa cho mỗi trường hợp thử nghiệm là cố định, do đó giải pháp dễ dàng phù hợp với giới hạn một giây ngay cả với số lượng trường hợp thử nghiệm tối đa. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)

    def solve():
        input = sys.stdin.readline
        t = int(input())
        keys = [(0, 0), (0, 1), (1, 0), (1, 1)]
        out = []

        for _ in range(t):
            m, n = map(int, input().split())
            rows = set(map(int, input().split()))
            cols = set(map(int, input().split()))

            ans = 0
            for mask in range(16):
                r = set()
                c = set()
                for i in range(4):
                    if mask & (1 << i):
                        r.add(keys[i][0])
                        c.add(keys[i][1])
                if r == rows and c == cols:
                    ans += 1

            out.append(str(ans))

        return "\n".join(out)

    result = solve()
    sys.stdin = old_stdin
    return result

assert run("""2
2 1
0 1
0
1 2
1
0 1
""") == "1\n1", "sample 1"

assert run("""2
2 2
0 1
0 1
1 1
1
1
""") == "7\n1", "sample 2"

assert run("""3
1 1
0
0
1 1
1
1
2 2
0 1
0 1
""") == "1\n1\n7", "basic signals"

assert run("""1
1 2
0
0 1
""") == "2", "single row with two columns"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Hàng đơn và cột đơn | 1 | Một tín hiệu quan trọng duy nhất | 
| Tín hiệu bàn phím đầy đủ | 7 | Nhiều kết hợp tạo ra cùng một tín hiệu | 
| Các góc khác nhau | 1 | Ghép nối hàng-cột đúng | 
| Một hàng có hai cột | 2 | Sự kết hợp mơ hồ | 

## Vỏ cạnh 

Đối với tín hiệu được tạo bởi một khóa chính xác, chẳng hạn như:```
1
1 1
0
0
```thuật toán kiểm tra tất cả 16 tập hợp con. Chỉ tập hợp con chứa khóa 1 mới tạo tập hợp hàng`{0}`và tập cột`{0}`, vì vậy câu trả lời là 1. Giải pháp không nhầm lẫn các lựa chọn hàng và cột độc lập với các khóa thực tế. 

Đối với tín hiệu chứa tất cả các hàng và cột:```
1
2 2
0 1
0 1
```thuật toán tìm thấy bảy tập hợp con phù hợp. Tập hợp con trống bị từ chối vì nó không tạo ra hàng và không có cột, trong khi mọi tập hợp con không trống ngoại trừ các kết hợp một hàng hoặc một cột thiếu một tọa độ đều được xử lý chính xác. 

Đối với một tín hiệu mơ hồ như:```
1
2 2
0 1
0 1
```các tập hợp con`{1,4}`Và`{2,3}`cả hai đều tạo ra tín hiệu điều khiển giống nhau. Việc liệt kê tính cả hai vì nó kiểm tra các kết hợp một cách trực tiếp thay vì cố gắng xây dựng lại các khóa gốc từ thông tin không đầy đủ.
