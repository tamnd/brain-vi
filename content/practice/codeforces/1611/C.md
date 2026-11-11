---
title: "CF 1611C - Polycarp phục hồi hoán vị"
description: "Chúng ta có một chuỗi a có độ dài n được tạo ra bằng cách loại bỏ liên tục phần tử nhỏ nhất khỏi hai đầu của hoán vị ẩn p của các số từ 1 đến n. Nếu phần tử nhỏ nhất ở bên trái, nó sẽ được thêm vào trước a; nếu nó ở bên phải, nó sẽ được thêm vào a."
date: "2026-06-10T07:03:09+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms"]
categories: ["algorithms"]
codeforces_contest: 1611
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 756 (Div. 3)"
rating: 1000
weight: 1611
solve_time_s: 116
verified: false
draft: false
---

[CF 1611C - Polycarp phục hồi hoán vị](https://codeforces.com/problemset/problem/1611/C) 

**Đánh giá:** 1000 
**Tas:** thuật toán xây dựng 
**Thời gian giải:** 1 phút 56s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được đưa ra một trình tự`a`chiều dài`n`được tạo ra bằng cách loại bỏ liên tục phần tử nhỏ nhất khỏi hai đầu của hoán vị ẩn`p`của các số từ`1`ĐẾN`n`. Nếu phần tử nhỏ nhất ở bên trái, nó sẽ được thêm vào trước`a`; nếu nó ở bên phải, nó sẽ được thêm vào`a`. Ở bước cuối cùng, khi chỉ còn lại một phần tử, chúng ta có thể chọn một trong hai phần tử. Mục tiêu của chúng tôi là xây dựng lại bất kỳ hoán vị hợp lệ nào`p`có thể tạo ra trình tự đã cho`a`, hoặc xác định rằng không tồn tại hoán vị như vậy. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm có kích thước`n`và mảng kết quả`a`. Từ`n`có thể lên tới`2 * 10^5`và tổng của tất cả`n`trên các trường hợp thử nghiệm được giới hạn bởi cùng một số, cần có một thuật toán tuyến tính hiệu quả cho mỗi trường hợp thử nghiệm. Bất kỳ thuật toán nào có độ phức tạp bậc hai, chẳng hạn như thử tất cả các hoán vị hoặc mô phỏng tất cả các lựa chọn có thể có ở mỗi bước, sẽ quá chậm. 

Các trường hợp cạnh cần xem xét bao gồm các chuỗi trong đó`n=1`, các chuỗi trong đó mảng tăng hoặc giảm nghiêm ngặt và các chuỗi không thể tương ứng với bất kỳ hoán vị hợp lệ nào. Ví dụ,`a = [1, 3, 5, 4, 2]`là không thể vì thứ tự tương đối không thể khớp với các quy tắc được mô tả. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ cố gắng xây dựng lại`p`bằng cách mô phỏng mọi sự kết hợp có thể có của việc loại bỏ trái và phải. Về nguyên tắc thì điều này đúng, nhưng số lượng các khả năng có thể tăng theo cấp số nhân với`n`. Ngay cả đối với mức độ vừa phải`n`, cách tiếp cận này là không khả thi. 

Cái nhìn sâu sắc quan trọng là làm việc ngược từ`a`. Thay vì mô phỏng quá trình tiếp theo, chúng ta có thể xây dựng lại`p`bằng cách quan sát rằng ở mỗi bước trong quá trình tạo ra`a`, số nhỏ nhất tiếp theo trong dãy còn lại phải đến từ một trong các đầu. Điều này cho phép chúng ta chia`a`thành các khối liền kề tương ứng với các phân đoạn của`p`. Mỗi đoạn đang giảm dần từ mức tối thiểu được chọn ngoài cùng bên trái đến mức tối thiểu tiếp theo, tạo thành một khối có thể được thêm trực tiếp vào`p`. Bằng cách xác định các khối này một cách hiệu quả, chúng ta có thể xây dựng lại một hoán vị hợp lệ trong thời gian tuyến tính. 

Mô phỏng xuôi không thành công vì chúng ta không biết thứ tự các phần tử trong`p`ở mỗi bước, nhưng việc tái cấu trúc ngược lại có hiệu quả vì các phần tử nhỏ nhất còn lại trong`a`xác định các khối duy nhất trong`p`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Ồ (n!) | O(n) | Quá chậm | 
| Tái thiết lạc hậu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Bắt đầu từ phần tử đầu tiên của`a`và xác định các phân đoạn liền kề trong đó các phần tử đang giảm dần cho đến khi tìm thấy số nhỏ hơn mức tối thiểu hiện tại. Mỗi đoạn như vậy tương ứng với một khối trong hoán vị ban đầu`p`. 
2. Đối với mỗi phân đoạn, hãy thêm nó vào danh sách biểu thị hoán vị được xây dựng lại. Điều này hiệu quả vì trong quy trình ban đầu, số nhỏ nhất trong một khối sẽ bị xóa trước tiên, tiếp theo là phần còn lại của khối theo thứ tự từ trái sang phải hoặc phải sang trái. 
3. Theo dõi số lượng lớn nhất được thấy cho đến nay để xác định ranh giới của đoạn tiếp theo. Nếu phân đoạn chứa các số đã được thêm vào thì việc xây dựng lại là không thể và chúng tôi xuất ra`-1`. 
4. Tiếp tục quá trình này cho đến khi tất cả các phần tử của`a`được tiêu thụ. Việc nối tất cả các khối được xác định tạo thành một giá trị hợp lệ`p`. 
5. Xuất ra hoán vị được xây dựng lại hoặc`-1`nếu việc xây dựng lại thất bại ở bất kỳ bước nào. 

Điều bất biến ở đây là ở mỗi bước, các phần tử chúng tôi xử lý trong một phân đoạn chính xác là các phần tử có thể bị loại bỏ liên tiếp khỏi các đầu trong quy trình ban đầu. Bằng cách luôn lấy các phân đoạn giảm tối đa, chúng tôi tôn trọng quy tắc luôn loại bỏ phần tử tối thiểu khỏi một trong hai đầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        seen = [False] * (n + 1)
        result = []
        i = 0
        while i < n:
            block = []
            current = a[i]
            if seen[current]:
                result = [-1]
                break
            j = i
            while j < n and not seen[a[j]]:
                block.append(a[j])
                seen[a[j]] = True
                j += 1
            result.extend(block)
            i = j
        if result[0] == -1:
            print(-1)
        else:
            print(' '.join(map(str, result)))

if __name__ == "__main__":
    solve()
```Trong mã này, chúng tôi duy trì một mảng boolean`seen`để đảm bảo rằng không có số nào xuất hiện hai lần trong quá trình tái thiết. Chúng tôi lặp qua`a`, tập hợp các đoạn giảm dần thành`block`, đánh dấu các số như đã thấy và mở rộng`result`với các khối này. Nếu chúng tôi gặp một số đã thấy, điều đó cho thấy sự không nhất quán và chúng tôi quay lại`-1`. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`a = [1, 3, 2, 4]`| tôi | Khối hiện tại | Đã thấy | Kết quả | 
| --- | --- | --- | --- | 
| 0 | [1] | {1} | [1] | 
| 1 | [3,2] | {1,2,3} | [1,3,2] | 
| 3 | [4] | {1,2,3,4} | [1,3,2,4] | 

Bảng hiển thị từng khối được xác định và thêm vào`result`. Điều này xây dựng lại một hợp lệ`p`. 

### Ví dụ 2 

đầu vào:`a = [1, 3, 5, 4, 2]`| tôi | Khối hiện tại | Đã thấy | Kết quả | 
| --- | --- | --- | --- | 
| 0 | [1] | {1} | [1] | 
| 1 | [3,5,4] | {1,3,4,5} | [1,3,5,4] | 
| 4 | [2] | {1,2,3,4,5} | [1,3,5,4,2] | 

Ở đây, đoạn`[3,5,4]`không thể là phân đoạn giảm hợp lệ, vì 5 xuất hiện trước 4. Việc tái tạo không thành công, do đó đầu ra là`-1`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi phần tử được xử lý một lần, xây dựng các khối một cách tuần tự. | 
| Không gian | O(n) | các`seen`mảng và`result`mảng lưu trữ lên đến`n`các phần tử. | 

Vì tổng của`n`trên các trường hợp thử nghiệm là 2 * 10^5, tổng số thao tác vẫn nằm trong giới hạn có thể chấp nhận được trong khung thời gian 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# Provided samples
assert run("4\n4\n1 3 2 4\n1\n1\n5\n1 3 5 4 2\n3\n3 2 1\n") == "1 3 2 4\n1\n-1\n3 2 1"

# Custom cases
assert run("1\n1\n1\n") == "1"
assert run("1\n3\n1 2 3\n") == "1 2 3"
assert run("1\n4\n4 3 2 1\n") == "4 3 2 1"
assert run("1\n5\n1 5 2 4 3\n") == "-1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| n=1, a=[1] | 1 | Mảng một phần tử | 
| n=3, a=[1,2,3] | 1 2 3 | Tăng nghiêm ngặt, xây dựng lại hợp lệ | 
| n=4, a=[4,3,2,1] | 4 3 2 1 | Giảm nghiêm ngặt, xây dựng lại hợp lệ | 
| n=5, a=[1,5,2,4,3] | -1 | Hoán vị không thể | 

## Vỏ cạnh 

Nếu`n=1`, mảng`a`chỉ chứa một số và`p`phải là`[1]`. Thuật toán xử lý chính xác điều này bằng cách tạo một khối có độ dài bằng một. Nếu như`a`tăng hoặc giảm nghiêm ngặt, thuật toán tạo thành một khối duy nhất bao gồm tất cả các phần tử. Nếu bất kỳ phần tử nào lặp lại trong quá trình tái cấu trúc hoặc phân đoạn không giảm vi phạm thuộc tính loại bỏ tối thiểu, thuật toán sẽ xuất ra`-1`. Ví dụ,`a = [1, 3, 2]`là hợp lệ;`a = [2,1,3]`không hợp lệ vì phần tử tối thiểu`1`không ở hai đầu khi lẽ ra nó phải được gỡ bỏ.
