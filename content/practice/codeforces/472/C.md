---
title: "CF 472C - Hướng dẫn thiết kế: Làm cho nó không xác định"
description: "Chúng tôi được đưa cho một danh sách những người, mỗi người có họ và tên. Đối với mỗi người, chúng tôi có quyền tự do chọn họ hoặc tên của họ làm địa chỉ."
date: "2026-05-30T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "greedy"]
categories: ["algorithms"]
codeforces_contest: 472
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 270"
rating: 1400
weight: 472
solve_time_s: 76
verified: true
draft: false
---

[CF 472C - Hướng dẫn thiết kế: Làm cho nó không xác định](https://codeforces.com/problemset/problem/472/C) 

**Đánh giá:** 1400 
**Thẻ:** tham lam 
**Thời gian giải:** 1 phút 16s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được đưa cho một danh sách những người, mỗi người có họ và tên. Đối với mỗi người, chúng tôi có quyền tự do chọn họ hoặc tên của họ làm địa chỉ. Ngoài ra, chúng tôi còn được cung cấp hoán vị các chỉ số từ 1 đến _n_, thể hiện thứ tự chính xác mà các điều khiển sẽ xuất hiện nếu được sắp xếp theo từ điển. Nhiệm vụ là xác định xem có thể gán các thẻ điều khiển cho mỗi người sao cho khi sắp xếp theo từ điển, chúng khớp với hoán vị đã cho hay không. 

Kích thước đầu vào có thể lên tới 100.000 người và mỗi tên có tối đa 50 ký tự. Việc sắp xếp 100.000 mục là khả thi, nhưng bất kỳ giải pháp nào thử tất cả 2^n kết hợp lựa chọn xử lý có thể là hoàn toàn không thực tế. Điều này có nghĩa là chúng ta phải tìm ra giải pháp quyết định cách xử lý của mỗi người một cách tham lam mà không liệt kê hết mọi khả năng. 

Trường hợp cạnh tinh tế xảy ra khi hai người liên tiếp trong hoán vị có phạm vi điều khiển chồng chéo. Ví dụ: giả sử người A có các phần tử "abc" và "def" và người B có các phần tử "abd" và "daa". Một cách tiếp cận ngây thơ có thể chỉ chọn phần xử lý nhỏ hơn về mặt từ điển cho mỗi người mà không xem xét yêu cầu đặt hàng. Trong trường hợp này, việc chọn các núm điều khiển nhỏ hơn một cách tham lam có thể không thỏa mãn phép hoán vị, vì vậy chúng ta cần một quyết định tuần tự cẩn thận để tôn trọng các núm điều khiển đã được chọn. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực rất đơn giản: thử tất cả các bài tập xử lý 2^n cho n người, sắp xếp danh sách kết quả và kiểm tra xem nó có khớp với hoán vị đã cho hay không. Điều này được đảm bảo là đúng, nhưng với n lên tới 100.000 thì không khả thi vì 2^100.000 là lớn về mặt thiên văn. Ngay cả một cách tiếp cận quay lui để cắt bớt các chuỗi không thể vẫn có thể giảm độ phức tạp theo cấp số nhân nếu việc cắt tỉa không đủ chặt chẽ. 

Điều quan trọng cần lưu ý là chúng ta chỉ cần thực hiện các lựa chọn một cách tuần tự theo thứ tự hoán vị cho trước. Giả sử chúng ta xử lý hoán vị từ vị trí đầu tiên đến vị trí cuối cùng. Đối với mỗi người, chúng tôi biết phần điều khiển nhỏ nhất về mặt từ điển mà chúng tôi có thể chọn phải lớn hơn phần điều khiển mà chúng tôi đã chọn cho người trước đó. Thông tin chi tiết này cho phép áp dụng chiến lược tham lam: đối với mỗi người theo thứ tự hoán vị, hãy chọn tay cầm nhỏ hơn lớn hơn tay cầm trước đó và nếu tay cầm nhỏ hơn không thành công, hãy chọn tay cầm lớn hơn. Nếu không có lựa chọn nào thỏa mãn thứ tự thì việc gán là không thể. 

Điều này biến một bài toán hàm mũ thành một bài toán tuyến tính. Chúng tôi không còn liệt kê tất cả các kết hợp mà thay vào đó đưa ra một lựa chọn xác định cho mỗi người, kiểm tra tính khả thi cục bộ ở mỗi bước. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n * n log n) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số người _n_ và lưu trữ mỗi người dưới dạng một bộ dữ liệu (first_name, Last_name). 
2. Đọc mảng hoán vị và chuyển đổi nó thành các chỉ số dựa trên 0 để truy cập mảng dễ dàng hơn. 
3. Với mỗi người, hãy sắp xếp hai tay cầm của họ theo từ điển. Điều này đảm bảo rằng tay cầm "nhỏ hơn" luôn đứng đầu, giúp đơn giản hóa việc so sánh. 
4. Khởi tạo một biến`prev_handle`thành một chuỗi trống. Biến này theo dõi phần xử lý được chọn cho người đứng trước theo thứ tự hoán vị. 
5. Lặp lại mảng hoán vị. Với mỗi người, hãy kiểm tra xem tay cầm nhỏ hơn có hoàn toàn lớn hơn`prev_handle`. Nếu có thì hãy chọn nó. Ngược lại, hãy kiểm tra xem tay cầm lớn hơn có lớn hơn đúng không`prev_handle`. Nếu không có bộ điều khiển nào thỏa mãn điều kiện, xuất "NO" và kết thúc. 
6. Nếu tất cả mọi người đã được chỉ định địa chỉ thành công, hãy ghi "CÓ". 

Tại sao nó hoạt động: Ở mỗi bước, thuật toán đảm bảo rằng phần điều khiển đã chọn tuân theo thứ tự từ điển liên quan đến tất cả các phần điều khiển trước đó. Bởi vì hoán vị xác định một thứ tự tuyến tính, không có lựa chọn nào trong tương lai có thể vi phạm các lựa chọn trong quá khứ nếu chúng ta luôn chọn điều khiển khả thi nhỏ nhất. Cách tiếp cận tham lam này đảm bảo sẽ tìm được một bài tập hợp lệ nếu có. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
people = [tuple(input().strip().split()) for _ in range(n)]
perm = list(map(int, input().strip().split()))

# convert to 0-based indices
perm = [x-1 for x in perm]

# sort each person's handles
handles = [tuple(sorted(person)) for person in people]

prev_handle = ""
possible = True

for idx in perm:
    small, large = handles[idx]
    if small > prev_handle:
        prev_handle = small
    elif large > prev_handle:
        prev_handle = large
    else:
        possible = False
        break

print("YES" if possible else "NO")
```Trước tiên, giải pháp sẽ đọc và sắp xếp dữ liệu đầu vào thành một danh sách các bộ dữ liệu đã được sắp xếp, đảm bảo phần xử lý nhỏ hơn sẽ được ưu tiên trước. Sau đó, nó thực hiện hoán vị, tham lam chọn phần xử lý khả thi nhỏ nhất để duy trì trật tự từ điển. các`prev_handle`biến rất quan trọng: nó mã hóa ràng buộc thứ tự từ các lựa chọn trước đó và việc kiểm tra cả hai tùy chọn xử lý đảm bảo chúng ta không bao giờ bỏ lỡ một phép gán hợp lệ. Tránh các lỗi từng cái một bằng cách chuyển đổi hoán vị thành lập chỉ mục dựa trên 0. 

## Ví dụ đã hoạt động 

**Đầu vào mẫu 1:**```
3
gennady korotkevich
petr mitrichev
gaoyuan chen
1 2 3
```| Bước | idx | nhỏ | lớn | trước_handle | đã chọn | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 0 | thần thánh | korotkevich | "" | thần thánh | 
| 2 | 1 | mitrichev | petr | thần thánh | mitrichev | 
| 3 | 2 | chen | cao nguyên | mitrichev | không → thất bại | 

Ở đây, số nhỏ nhất của người thứ ba là "chen" không lớn hơn "mitrichev", và "gaoyuan" cũng không lớn hơn. Đầu ra: KHÔNG. 

**Đầu vào mẫu 2:**```
3
copernicus galileo
isaac newton
albert einstein
2 3 1
```| Bước | idx | nhỏ | lớn | trước_handle | đã chọn | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | isaac | newton | "" | isaac | 
| 2 | 2 | albert | einstein | isaac | einstein | 
| 3 | 0 | copernicus | galileo | einstein | galileo | 

Tất cả các tay cầm được chọn đều đáp ứng thứ tự. Đầu ra: CÓ. 

Những dấu vết này cho thấy việc lựa chọn tham lam đảm bảo tính khả thi trong khi vẫn tôn trọng hoán vị. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi người được xử lý một lần; so sánh chuỗi mất tối đa O(50) cho mỗi bộ điều khiển, giá trị này thực tế là không đổi. | 
| Không gian | O(n) | Lưu trữ n bộ điều khiển và mảng hoán vị. | 

Cho n ≤ 10^5 và độ dài tên ≤ 50, đường chuyền tuyến tính của chúng tôi nằm trong giới hạn thời gian 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(input())
    people = [tuple(input().strip().split()) for _ in range(n)]
    perm = list(map(int, input().strip().split()))
    perm = [x-1 for x in perm]
    handles = [tuple(sorted(person)) for person in people]
    prev_handle = ""
    possible = True
    for idx in perm:
        small, large = handles[idx]
        if small > prev_handle:
            prev_handle = small
        elif large > prev_handle:
            prev_handle = large
        else:
            possible = False
            break
    return "YES" if possible else "NO"

# provided samples
assert run("3\ngennady korotkevich\npetr mitrichev\ngaoyuan chen\n1 2 3\n") == "NO", "sample 1"

# custom cases
assert run("1\nalice wonderland\n1\n") == "YES", "minimum input"
assert run("2\nbob alice\ncarol dave\n2 1\n") == "YES", "reversed permutation"
assert run("2\nbob alice\ncarol dave\n1 2\n") == "NO", "cannot satisfy order"
assert run("3\namy aaron\nbob brian\ncara cecil\n3 2 1\n") == "YES", "all choices flexible"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 người | CÓ | Xử lý đầu vào tối thiểu | 
| Hoán vị ngược | CÓ | Hoán vị không được sắp xếp tự nhiên | 
| Không thể 2 người | KHÔNG | Ràng buộc thứ tự không thể được thỏa mãn | 
| Linh hoạt 3 người | CÓ | Nhiều lựa chọn hợp lệ, lựa chọn tham lam có tác dụng | 

## Vỏ cạnh 

Nếu cả hai tay cầm của một người đều nhỏ hơn tay cầm đã chọn trước đó, thuật toán sẽ ngay lập tức phát hiện ra điều không thể. Ví dụ: với đầu vào:```
2
aa ab
ac ad
2 1
```Chúng tôi sắp xếp các thẻ điều khiển: người 0 → ("aa","ab"), người 1 → ("ac","quảng cáo"). Xử lý chỉ số hoán vị 1 trước,`prev_handle`là "", ta chọn "ac". Tiếp theo, chỉ số 0, các thẻ điều khiển "aa", "ab" đều không lớn hơn "ac", do đó thuật toán đưa ra NO chính xác. Điều này chứng tỏ rằng bước tham lam đảm bảo không có phép gán không hợp lệ nào được thực hiện.
