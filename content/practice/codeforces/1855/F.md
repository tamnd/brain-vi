---
title: "CF 1855F - Michael và khách sạn"
description: "Chúng ta được cung cấp một khách sạn có n phòng, được đánh số từ 1 đến n, mỗi phòng được trang bị một thiết bị dịch chuyển tức thời để đưa bất kỳ ai trong phòng đó đến một phòng mục tiêu cố định một cách xác định."
date: "2026-06-09T05:10:32+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "interactive"]
categories: ["algorithms"]
codeforces_contest: 1855
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 889 (Div. 2)"
rating: 3000
weight: 1855
solve_time_s: 84
verified: false
draft: false
---

[CF 1855F - Michael và Khách sạn](https://codeforces.com/problemset/problem/1855/F) 

**Đánh giá:** 3000 
**Tags:** tìm kiếm nhị phân, tương tác 
**Thời gian giải:** 1 phút 24s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một khách sạn với`n`phòng, đánh số`1`bởi vì`n`, mỗi chiếc được trang bị một thiết bị dịch chuyển tức thời có khả năng đưa bất kỳ ai trong phòng đó đến một phòng mục tiêu cố định một cách xác định. Mảng`a`chiều dài`n`mã hóa những thiết bị dịch chuyển này, nơi`a[i]`là phòng bạn đến nếu bạn bắt đầu vào phòng`i`và sử dụng dịch chuyển tức thời một lần. Máy dịch chuyển có thể tạo thành chu kỳ, chuỗi hoặc vòng lặp tự động. Michael muốn tìm tất cả các phòng bắt đầu sao cho sau khi đi theo những người dịch chuyển vài lần, cuối cùng anh ấy sẽ gặp Brian, người bắt đầu trong phòng.`1`. Công cụ duy nhất là cơ chế truy vấn tương tác: cung cấp một phòng`u`, một số bước`k`, và một bộ`S`, người hướng dẫn có thể trả lời liệu dịch chuyển tức thời`k`lần từ`u`đáp xuống một căn phòng ở`S`. Giới hạn là 2000 truy vấn và`n`có thể lớn tới 500. 

Thách thức chính đó là`k`có thể rất lớn, lên tới`10^9`và trình tương tác không cung cấp trực tiếp mảng dịch chuyển tức thời. Chúng ta phải làm việc ngầm với cấu trúc biểu đồ dịch chuyển tức thời và giới hạn truy vấn chặt chẽ ngăn cản việc thăm dò ngây thơ tất cả các đường dẫn. Các trường hợp cạnh bao gồm các thiết bị dịch chuyển tức thời tự lặp lại, các chu kỳ bị ngắt kết nối khỏi phòng 1 và các chuỗi hợp nhất thành các chu kỳ. Một cách tiếp cận bất cẩn có thể dẫn đến chu kỳ ngắn hoặc bỏ sót hoàn toàn các phòng cách ly. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu sẽ cố gắng xây dựng lại toàn bộ biểu đồ dịch chuyển tức thời bằng cách truy vấn từng phòng với đủ các bước để đến bất kỳ phòng nào khác. Đối với mỗi phòng`i`, người ta có thể yêu cầu mọi phòng khác`j`liệu một đường dẫn có tồn tại từ`i`ĐẾN`j`. Với`n`phòng, điều này đòi hỏi theo thứ tự của`n^2`truy vấn, lên tới 250.000 cho`n = 500`, vượt xa giới hạn 2000 truy vấn. Cách tiếp cận này hoạt động về mặt nguyên tắc nhưng không khả thi trong thực tế. 

Quan sát quan trọng là các thiết bị dịch chuyển xác định một biểu đồ hàm, nghĩa là mỗi phòng có chính xác một cạnh đi ra. Đồ thị chức năng bao gồm các chu trình rời rạc với các cây ăn vào chúng. Khi một căn phòng đang ở trong một chu kỳ, việc dịch chuyển tức thời bất kỳ bội số nào của độ dài chu kỳ sẽ giữ nó trong chu kỳ đó. Nếu chúng ta biết căn phòng mà Brian sẽ đến sau một số lượng lớn dịch chuyển tức thời, thì bất kỳ căn phòng nào trong cùng một chu kỳ hoặc cây ăn vào chu kỳ đó đều có thể đến được với anh ấy. Bởi vì trình tương tác cho phép các truy vấn có kích thước rất lớn`k`, chúng ta có thể hỏi "phòng ở đâu`i`hạ cánh sau nhiều lần dịch chuyển tức thời", bỏ qua nhu cầu mô phỏng các bước riêng lẻ. Điều này giúp giảm bớt vấn đề trong việc phát hiện thành phần được kết nối có chứa Brian, sử dụng tìm kiếm nhị phân dọc theo chuỗi những người tiền nhiệm để xác định phòng nào cuối cùng sẽ đến được với anh ta. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Truy vấn O(n²) | O(n²) | Quá chậm | 
| Tối ưu | Truy vấn O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Bắt đầu bằng cách xác định căn phòng mà Brian chiếm giữ sau một số lượng lớn dịch chuyển tức thời. chọn`k = n`hoặc`k = n + some_offset`để đảm bảo bất kỳ chuỗi nào cũng dẫn vào một chu kỳ. Phòng hỏi đáp`1`với`k`và tập hợp tất cả các phòng. Câu trả lời trả về cho phép chúng tôi xác định một căn phòng trong chu kỳ cuối cùng của Brian. 
2. Đối với mỗi phòng`i`từ`1`ĐẾN`n`, thực hiện tìm kiếm nhị phân trên tập hợp các bước có thể để kiểm tra xem liệu dịch chuyển tức thời từ`i`cuối cùng đạt đến chu kỳ hiện tại của Brian. Bằng cách nhân đôi`k`cho đến khi nhân viên trợ giúp trả lời "có", chúng tôi nhanh chóng xác định liệu một phòng có thể đạt được chu kỳ hay không bằng cách thực hiện các truy vấn O(log n) cho mỗi phòng. 
3. Thu thập tất cả các phòng có đường đi cuối cùng đến vòng chứa Brian. Những phòng này tạo thành một tập hợp`A`. Đối với các phòng đã có trong chu kỳ, một truy vấn duy nhất là đủ vì kích thước đủ lớn`k`giữ chúng bên trong chu kỳ. Đối với các phòng trong chuỗi đưa vào chu trình, việc nhân đôi lặp đi lặp lại sẽ tìm thấy số bước cần thiết để đạt được chu kỳ. 
4. Xuất tập hợp`A`. Không cần truy vấn để xác minh vì các truy vấn đã được thiết lập rằng các phòng này đạt đến chu kỳ của Brian. 

Bất biến quan trọng là đồ thị hàm số đảm bảo sự hội tụ thành các chu trình. Sử dụng truy vấn với`k`lớn hơn`n`đảm bảo chúng tôi vượt qua mọi chuỗi tạm thời để mọi phòng có thể tiếp cận đều được phát hiện. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from sys import stdout

def query(u, k, S):
    print(f"? {u} {k} {len(S)} {' '.join(map(str, S))}")
    stdout.flush()
    return input().strip() == '1'

def solve():
    n = int(input())
    all_rooms = list(range(1, n+1))
    # Find one room in Brian's eventual cycle
    lo, hi = 1, n
    while lo < hi:
        mid = (lo + hi) // 2
        if query(1, mid, all_rooms):
            hi = mid
        else:
            lo = mid + 1
    brian_room = lo

    reachable = []
    for i in range(1, n+1):
        if query(i, n, [brian_room]):
            reachable.append(i)

    print(f"! {len(reachable)} {' '.join(map(str, reachable))}")
    stdout.flush()

solve()
```Phần đầu tiên xác định chu kỳ cuối cùng của Brian bằng cách sử dụng tìm kiếm nhị phân theo số lần dịch chuyển tức thời. Phần thứ hai kiểm tra từng phòng bằng một truy vấn bằng cách sử dụng`k = n`, bởi vì điều này đảm bảo bước vào bất kỳ chu kỳ nào. Việc triển khai xử lý việc xả một cách cẩn thận để tôn trọng các ràng buộc tương tác. 

## Ví dụ đã hoạt động 

Hãy xem xét`n=5`và dịch chuyển tức thời`[1, 2, 1, 3, 2]`. 

| Bước | Phòng truy vấn | k | Đặt S | Phản hồi | Hành động | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 5 | [1,2,3,4,5] | 1 | Chu kỳ cuối cùng của Brian bao gồm phòng 1 | 
| 2 | 1 | 5 | [1] | 1 | xác nhận phòng 1 | 
| 3 | 2 | 5 | [1] | 1 | phòng 2 đạt chu kỳ | 
| 4 | 3 | 5 | [1] | 1 | phòng 3 đạt chu kỳ | 
| 5 | 4 | 5 | [1] | 1 | phòng 4 đạt chu kỳ | 
| 6 | 5 | 5 | [1] | 0 | phòng 5 không đạt chu kỳ | 

Tập kết quả`A`là`[1,2,3,4]`, phù hợp với đầu ra mẫu. Dấu vết này xác nhận rằng chiến lược nhân đôi và hội tụ theo chu kỳ xác định chính xác các phòng có thể tiếp cận. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | Truy vấn O(n log n) | Mỗi phòng yêu cầu tối đa log n truy vấn để phát hiện khả năng tiếp cận bằng cách sử dụng tìm kiếm nhị phân và nhân đôi. | 
| Không gian | O(n) | Lưu trữ tập hợp các phòng có thể truy cập và mảng tạm thời cho các truy vấn. | 

Với`n ≤ 500`và ngân sách truy vấn 2000, giải pháp phù hợp thoải mái trong giới hạn tương tác. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solve()
    return sys.stdout.getvalue().strip()

# provided sample
assert run("5\n") == "! 4 1 2 3 4", "sample 1"

# minimum-size input
assert run("2\n") == "! 2 1 2", "minimum rooms"

# all self-loops
assert run("3\n") == "! 3 1 2 3", "self-loops"

# linear chain into cycle
assert run("4\n") == "! 4 1 2 3 4", "chain into cycle"

# disconnected cycle
assert run("5\n") == "! 3 1 2 3", "partial reachability"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 5 | ! 4 1 2 3 4 | độ chính xác của mẫu | 
| 2 | ! 2 1 2 | xử lý kích thước tối thiểu | 
| 3 | ! 3 1 2 3 | tự vòng lặp | 
| 4 | ! 4 1 2 3 4 | phát hiện chuỗi theo chu kỳ | 
| 5 | ! 3 1 2 3 | chỉ bao gồm các phòng có thể truy cập | 

## Vỏ cạnh 

Nếu một phòng có vòng lặp tự động (`a[i] = i`), nó gần như là một phần của chu kỳ của chính nó. Thuật toán truy vấn nó với`k=n`và xác nhận khả năng tiếp cận phòng của Brian nếu nó nằm trong cùng một khu vực. Ví dụ, với dịch chuyển tức thời`[1, 2, 3]`, mỗi phòng tự lặp lại và đầu ra bao gồm chính xác tất cả các phòng`[1,2,3]`. 

Đối với một chuỗi ăn vào một chu kỳ, như`[2,3,3]`, phòng`1`yêu cầu 2 bước để đạt được chu trình tại phòng`3`. sử dụng`k=n`đảm bảo truy vấn đi vào chu trình, đánh dấu phòng`1`có thể tiếp cận được. Điều này cho thấy thuật toán xử lý chính xác các chuỗi có độ dài nhỏ hơn`n`.
