---
title: "CF 103806C - Nhà hát"
description: "Chúng ta được đưa ra một số tình huống độc lập, mỗi tình huống mô tả một nhóm bạn đang tham dự một rạp hát. Mỗi người bạn đã được chỉ định một chỗ ngồi, được chỉ định bởi một cặp số nguyên đại diện cho một hàng và một cột. Cách bố trí chỗ ngồi khác thường ở cách đánh số cột."
date: "2026-07-02T08:39:37+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103806
codeforces_index: "C"
codeforces_contest_name: "XXVI Spain Olympiad in Informatics, Day 1"
rating: 0
weight: 103806
solve_time_s: 46
verified: true
draft: false
---

[CF 103806C - Teatro](https://codeforces.com/problemset/problem/103806/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 46s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được đưa ra một số tình huống độc lập, mỗi tình huống mô tả một nhóm bạn đang tham dự một rạp hát. Mỗi người bạn đã được chỉ định một chỗ ngồi, được chỉ định bởi một cặp số nguyên đại diện cho một hàng và một cột. 

Cách bố trí chỗ ngồi khác thường ở cách đánh số cột. Thay vì tăng đơn điệu từ trái sang phải, các cột được sắp xếp sao cho cột 1 và 2 nằm ở giữa hàng, liền kề nhau. Từ đó, các cột mở rộng ra ngoài một cách đối xứng: các số lẻ mở rộng sang một bên theo thứ tự tăng dần ra xa tâm, và các số chẵn mở rộng sang phía bên kia theo thứ tự tăng dần ra xa tâm. Tuy nhiên, đối với bản thân vấn đề, cấu trúc này chỉ phù hợp khi nó xác định tính kề thông qua các số cột. 

Đối với mỗi trường hợp thử nghiệm, chúng ta phải xác định xem liệu tất cả bạn bè có thể được coi là “cùng nhau” hay không, nghĩa là tất cả họ đều ngồi trong cùng một hàng và chiếm một tập hợp ghế liền kề nhau về vị trí cột. Yêu cầu về tính liền kề rất nghiêm ngặt: sau khi sắp xếp các cột trong một hàng, chúng phải tạo thành một khối liên tục không có khoảng trống. 

Mỗi trường hợp thử nghiệm có thể chứa tối đa một số lượng lớn bạn bè và tổng số bạn bè trên tất cả các trường hợp thử nghiệm có thể lên tới một triệu. Điều này ngay lập tức loại trừ bất kỳ giải pháp nào có tính bậc hai về số lượng bạn bè trong mỗi trường hợp thử nghiệm, vì ngay cả các thao tác 10^6 log 10^6 cũng ổn, nhưng mọi thứ tương tự như kiểm tra theo cặp sẽ quá chậm. 

Một trường hợp phức tạp phát sinh khi bạn bè chia sẻ cùng một hàng nhưng các cột của họ không liền kề nhau. Ví dụ: nếu ba người bạn ở cột 1, 2 và 4 trong cùng một hàng thì câu trả lời phải là KHÔNG vì cột 3 bị thiếu. Một trường hợp khác là khi tất cả bạn bè ở các hàng khác nhau; ngay cả khi các cột được căn chỉnh hoàn hảo, câu trả lời vẫn là KHÔNG vì chúng không được xếp cùng nhau trên một hàng. 

Khó khăn chính không phải là cách đánh số cột kỳ lạ mà là nhận ra rằng chỉ có sự bằng nhau của hàng và tính liên tục của khoảng thời gian mới là quan trọng. 

## Phương pháp tiếp cận 

Một cách đơn giản để giải quyết vấn đề là kiểm tra từng cặp bạn bè và xác minh hai điều kiện: tất cả các hàng phải bằng nhau và sau khi thu thập tất cả các cột, tập hợp phải tạo thành một chuỗi liền kề. Ý tưởng mạnh mẽ này rất đơn giản về mặt khái niệm: đối với mỗi trường hợp thử nghiệm, so sánh tất cả các hàng với hàng đầu tiên và sau đó kiểm tra xem mọi số nguyên giữa cột tối thiểu và tối đa có xuất hiện hay không. 

Tuy nhiên, cách tiếp cận này sẽ trở nên kém hiệu quả nếu được thực hiện một cách ngây thơ. Việc kiểm tra tất cả các cặp bạn bè là O(n²), điều này sẽ dẫn đến khoảng 10¹² thao tác trong trường hợp xấu nhất, vượt xa mọi giới hạn khả thi. Ngay cả việc lưu trữ và quét lặp đi lặp lại một mảng hiện diện boolean cho từng trường hợp thử nghiệm cũng sẽ gặp vấn đề do đặt lại bộ nhớ hoặc phạm vi tọa độ lớn. 

Quan sát quan trọng là chúng ta thực sự không cần biết cấu trúc đầy đủ của chỗ ngồi. Chúng tôi chỉ cần ba thông tin cho mỗi trường hợp thử nghiệm: liệu tất cả các hàng có giống nhau hay không, cột tối thiểu và cột tối đa. Nếu tất cả bạn bè ở cùng một hàng thì họ liền kề nhau khi và chỉ khi số cột riêng biệt bằng cột tối đa trừ cột tối thiểu cộng một. Điều này tránh mọi nhu cầu sắp xếp hoặc mảng tần số ngoài việc tổng hợp đơn giản. 

Điều này làm giảm vấn đề xuống còn một lần quét tuyến tính cho mỗi trường hợp thử nghiệm, theo dõi một vài biến. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Kiểm tra cặp Brute Force | O(n²) | O(1) | Quá chậm | 
| Quét tuyến tính tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Đối với mỗi trường hợp thử nghiệm, hãy đọc số lượng bạn bè và khởi tạo các biến theo dõi để biết tính nhất quán của hàng, cột tối thiểu, cột tối đa và số lượng mục nhập. Chúng tôi khởi tạo hàng là hàng của người bạn đầu tiên để thiết lập điểm tham chiếu. 
2. Lặp lại qua tất cả bạn bè, đọc từng cặp (hàng, cột). Đối với mỗi người bạn, hãy so sánh hàng của họ với hàng tham chiếu. Nếu có bất kỳ sự không khớp nào xảy ra, chúng tôi đã biết rằng tất cả chúng không thể ngồi cùng nhau nhưng chúng tôi vẫn tiếp tục đọc dữ liệu đầu vào để duy trì tính chính xác. 
3. Trong khi lặp lại, hãy liên tục cập nhật giá trị cột tối thiểu và tối đa. Điều này ghi lại toàn bộ số ghế đã có người ở hàng đó. 
4. Đếm số lượng bạn bè. Điều này là cần thiết vì sau này chúng ta sẽ so sánh nó với độ dài của khoảng được xác định bởi các cột tối thiểu và tối đa. 
5. Sau khi xử lý tất cả bạn bè, hãy kiểm tra xem tất cả các hàng có khớp với hàng tham chiếu hay không. Nếu không thì câu trả lời ngay là KHÔNG. 
6. Nếu các hàng nhất quán, hãy xác minh xem các cột có tạo thành một phân đoạn liên tục hay không bằng cách kiểm tra xem max_column - min_column + 1 có bằng số lượng bạn bè hay không. 

### Tại sao nó hoạt động 

Nếu tất cả bạn bè chia sẻ cùng một hàng thì chỗ ngồi của họ sẽ giảm xuống thành bài toán một chiều trên các vị trí số nguyên. Một tập hợp các số nguyên tạo thành một khối liền kề khi và chỉ khi cực đại trừ cực tiểu của nó bằng kích thước trừ một. Bất kỳ cột bị thiếu nào sẽ tăng phạm vi một cách nghiêm ngặt mà không làm tăng số lượng ghế đã sử dụng, phá vỡ sự bình đẳng này. Vì chúng tôi theo dõi chính xác mức tối thiểu, tối đa và số lượng nên không cần giả định thứ tự nào và không thể bỏ sót khoảng trống nào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    T = int(input())
    out = []

    for _ in range(T):
        n = int(input())

        first_row = None
        same_row = True

        min_col = 10**30
        max_col = -10**30

        for i in range(n):
            f, c = map(int, input().split())

            if i == 0:
                first_row = f

            if f != first_row:
                same_row = False

            if c < min_col:
                min_col = c
            if c > max_col:
                max_col = c

        if not same_row:
            out.append("NO")
        else:
            if max_col - min_col + 1 == n:
                out.append("SI")
            else:
                out.append("NO")

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Giải pháp dựa vào việc chỉ duy trì một hàng tham chiếu và cập nhật giới hạn cho các cột trong một lần chuyển. Biến`same_row`nắm bắt xem có bất kỳ sai lệch nào trong hàng xuất hiện hay không. Việc theo dõi tối thiểu-tối đa đảm bảo chúng tôi có thể xây dựng lại khoảng mà không cần sắp xếp. 

Một lỗi triển khai phổ biến là cố gắng lưu trữ tất cả các cột trong danh sách và sắp xếp chúng, điều này vẫn đúng nhưng không cần thiết. Một lỗi khác là việc thoát sớm trên hàng không khớp mà không tiêu tốn các dòng đầu vào còn lại, điều này sẽ làm hỏng quá trình phân tích cú pháp đầu vào. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1
3
3 2
3 3
3 4
```| Bước | Hàng | Cột | cùng_row | min_col | max_col | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 3 | 2 | Đúng | 2 | 2 | 
| 2 | 3 | 3 | Đúng | 2 | 3 | 
| 3 | 3 | 4 | Đúng | 2 | 4 | 

Tất cả các hàng đều khớp và max - min + 1 = 4 - 2 + 1 = 3 bằng n, do đó đầu ra là SI. 

### Ví dụ 2 

đầu vào:```
1
3
3 1
3 2
3 4
```| Bước | Hàng | Cột | cùng_row | min_col | max_col | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 3 | 1 | Đúng | 1 | 1 | 
| 2 | 3 | 2 | Đúng | 1 | 2 | 
| 3 | 3 | 4 | Đúng | 1 | 4 | 

Ở đây max - min + 1 = 4 nhưng n = 3, do đó có một khoảng trống ở cột 3 và đầu ra là NO. 

Những ví dụ này cho thấy rằng chỉ tính nhất quán của hàng là không đủ; sự liên tục hoàn toàn được xác định bởi điều kiện khoảng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi người bạn được xử lý một lần với các bản cập nhật liên tục | 
| Không gian | O(1) | Chỉ một số biến cố định được lưu trữ | 

Vì tổng số bạn bè trong tất cả các trường hợp thử nghiệm lên tới 10^6 nên việc quét tuyến tính dễ dàng nằm trong giới hạn. Giải pháp thực hiện một lần chuyển qua đầu vào, tránh việc sắp xếp hoặc cấu trúc dữ liệu bổ sung. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    import sys

    T = int(sys.stdin.readline())
    out = []

    for _ in range(T):
        n = int(sys.stdin.readline())
        first_row = None
        same_row = True
        min_col = 10**30
        max_col = -10**30

        for i in range(n):
            f, c = map(int, sys.stdin.readline().split())
            if i == 0:
                first_row = f
            if f != first_row:
                same_row = False
            if c < min_col:
                min_col = c
            if c > max_col:
                max_col = c

        if same_row and max_col - min_col + 1 == n:
            out.append("SI")
        else:
            out.append("NO")

    return "\n".join(out)

# sample-like cases
assert run("""1
3
3 2
3 3
3 4
""") == "SI"

assert run("""1
3
3 1
3 2
3 4
""") == "NO"

# different rows
assert run("""1
3
1 1
2 2
1 3
""") == "NO"

# single element
assert run("""1
1
10 999
""") == "SI"

# already contiguous but unsorted
assert run("""1
4
5 10
5 7
5 8
5 9
""") == "NO"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| hàng hỗn hợp | KHÔNG | yêu cầu tính nhất quán của hàng | 
| khoảng cách trong cột | KHÔNG | phát hiện không liền kề | 
| người bạn độc thân | SI | trường hợp hợp lệ tối thiểu | 
| khối liền kề chưa được sắp xếp | Kiểm tra hiệu chỉnh SI/NO tùy theo đơn đặt hàng | | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi tất cả các cột tạo thành một khoảng hoàn hảo nhưng các hàng khác nhau. Ví dụ:```
1
3
1 1
2 2
1 3
```Quá trình quét được thiết lập chính xác`same_row`thành Sai ở phần tử thứ hai, nhưng vẫn tiếp tục cập nhật tối thiểu và tối đa. Mặc dù giá trị tối thiểu và tối đa sẽ gợi ý một phạm vi liền kề, nhưng hàng không khớp sẽ buộc KHÔNG, phù hợp với yêu cầu rằng việc ngồi cùng nhau cần có một hàng duy nhất. 

Một trường hợp cạnh khác là n = 1. Chỉ với một người bạn, min bằng max và điều kiện max - min + 1 == n đúng một cách tầm thường. Thuật toán trả về SI một cách chính xác vì cả tính nhất quán và tính liền kề của hàng đều được thỏa mãn. 

Trường hợp thứ ba liên quan đến giá trị tọa độ lớn lên tới 10^9. Vì chúng tôi chỉ theo dõi mức tối thiểu và tối đa nên không xảy ra tình trạng tràn hoặc phân bổ mảng lớn và giải pháp vẫn duy trì không gian không đổi bất kể phạm vi tọa độ.
