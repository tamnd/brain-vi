---
title: "CF 106057D - Zero không phải là một lựa chọn!"
description: "Chúng ta có một lưới hình chữ nhật trong đó mỗi ô chứa một số nguyên không âm. Từ mỗi hàng, chúng ta phải chọn chính xác một phần tử. Sau khi chọn một số trên mỗi hàng, chúng tôi tính toán AND theo bit của tất cả các giá trị đã chọn."
date: "2026-06-21T15:54:22+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106057
codeforces_index: "D"
codeforces_contest_name: "CoU CSE Fest 2025 - Inter University Programming Contest (Divisional)"
rating: 0
weight: 106057
solve_time_s: 41
verified: true
draft: false
---

[CF 106057D - Zero không phải là một lựa chọn!](https://codeforces.com/problemset/problem/106057/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 41s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một lưới hình chữ nhật trong đó mỗi ô chứa một số nguyên không âm. Từ mỗi hàng, chúng ta phải chọn chính xác một phần tử. Sau khi chọn một số trên mỗi hàng, chúng tôi tính toán AND theo bit của tất cả các giá trị đã chọn. Nhiệm vụ là xác định xem có tồn tại ít nhất một cách để chọn các giá trị như vậy sao cho kết quả AND cuối cùng hoàn toàn dương hay không. 

Bitwise AND chính xác là dương khi có ít nhất một vị trí bit trong đó mỗi số được chọn đều có số 1. Vì vậy, vấn đề thực sự đặt ra là liệu chúng ta có thể tìm thấy một bit có thể làm chung cho tất cả các phần tử hàng đã chọn cùng một lúc hay không. 

Lưới có thể đủ lớn nên việc thử tất cả các kết hợp của một phần tử trên mỗi hàng là không thể. Nếu có N hàng và M cột, cách tiếp cận bạo lực sẽ yêu cầu kiểm tra các lựa chọn M^N, tăng theo cấp số nhân và không khả thi ngay cả đối với các giá trị vừa phải như N = 20, M = 20. 

Cấu trúc của đầu vào gợi ý rằng chúng ta chỉ quan tâm đến tính khả dụng ở cấp độ bit trên mỗi hàng chứ không quan tâm đến các con số chính xác. Mỗi hàng đóng góp một tập hợp các bit có khả năng được cung cấp bởi ít nhất một phần tử trong hàng đó. 

Trường hợp cạnh khóa phát sinh khi một bit xuất hiện ở đâu đó trong mỗi hàng nhưng không xuất hiện trong cùng một chỉ mục cột trên các hàng. Ví dụ, hãy xem xét: 

Hàng 1: 2 (010), 1 (001) 

Hàng 2: 4 (100), 1 (001) 

Mỗi hàng chứa bit 0 (giá trị 1), do đó tồn tại một lựa chọn hợp lệ: chọn 1 từ cả hai hàng. Câu trả lời là CÓ. 

Nhưng nếu thay vào đó chúng ta có: 

Hàng 1: 2 (010) 

Hàng 2: 4 (100) 

Không có bit nào chung cho cả hai hàng, do đó, bất kể lựa chọn nào, AND đều trở thành 0. 

Điều này cho thấy vấn đề nằm ở sự giao nhau giữa tính khả dụng của bit trên mỗi hàng thay vì so sánh giá trị trực tiếp. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: liệt kê một phần tử trên mỗi hàng, tính AND và kiểm tra xem nó có khác 0 hay không. Điều này đúng vì nó khám phá tất cả các lựa chọn hợp lệ. Tuy nhiên, mỗi hàng có M lựa chọn nên tổng số kết hợp là M^N. Ngay cả với N = 10 và M = 10, con số này đã đạt đến 10^10 khả năng, vượt xa mọi giới hạn khả thi. 

Quan sát quan trọng là kết quả AND được xác định hoàn toàn bằng vị trí bit nào có mặt trong tất cả các số được chọn. Thay vì suy luận về các lựa chọn riêng lẻ, chúng ta có thể điều chỉnh lại vấn đề: chúng ta cần biết liệu có tồn tại một bit sao cho mỗi hàng có ít nhất một số chứa bit đó hay không. 

Điều này làm giảm vấn đề về tính khả dụng của bit trên mỗi hàng. Đối với mỗi hàng, chúng tôi tính toán một mặt nạ bit biểu thị tất cả các bit xuất hiện ở bất kỳ số nào trong hàng đó bằng cách sử dụng OR. Sau đó, chúng tôi kiểm tra những bit nào phổ biến trên tất cả các hàng bằng cách lấy AND của các mặt nạ này. Nếu mặt nạ giao nhau cuối cùng khác 0 thì ít nhất một bit có thể được giữ nguyên trong mỗi hàng, nghĩa là chúng ta có thể chọn các giá trị duy trì bit đó trong AND cuối cùng. 

Một quan điểm khác là kiểm tra từng bit một cách độc lập. Đối với mỗi vị trí bit, hãy kiểm tra xem mỗi hàng có chứa ít nhất một số có tập bit đó hay không. Nếu bất kỳ bit nào thỏa mãn điều kiện này, chúng ta biết ngay rằng có một lựa chọn hợp lệ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(M^N · N) | O(N) | Quá chậm | 
| Hàng HOẶC + Toàn cầu AND | O(NM) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Khởi tạo một mảng hoặc biến để lưu trữ mặt nạ bit trên mỗi hàng. Mỗi mặt nạ bắt đầu từ 0 vì ban đầu không có bit nào được ghi lại. 
2. Đối với mỗi hàng, quét tất cả các giá trị M và cập nhật mặt nạ hàng bằng cách sử dụng bitwise OR. Điều này ghi lại mọi bit xuất hiện trong ít nhất một phần tử của hàng đó. 
3. Duy trì mặt nạ chung được khởi tạo cho tất cả các bit được đặt (hoặc mặt nạ hàng đầu tiên). Sau khi xử lý từng hàng, hãy cập nhật nó bằng bitwise AND với mặt nạ hàng hiện tại. Bước này chỉ giữ lại các bit vẫn có thể có trên tất cả các hàng được xử lý. 
4. Sau khi xử lý tất cả các hàng, hãy kiểm tra xem mặt nạ chung có khác 0 hay không. Nếu nó khác 0, xuất CÓ; nếu không thì xuất ra NO. 

Lý do xây dựng mặt nạ OR theo hàng là vì chúng ta chỉ quan tâm liệu một bit có sẵn trong một hàng hay không, chứ không quan tâm đến phần tử cụ thể nào cung cấp nó. AND trên các hàng đảm bảo rằng chúng tôi chỉ giữ lại các bit tồn tại trong mỗi hàng. 

### Tại sao nó hoạt động 

Đối với bất kỳ vị trí bit b nào, thuật toán bảo toàn b trong mặt nạ hàng một cách chính xác khi tồn tại ít nhất một phần tử trong hàng đó với bit b được đặt. Sau khi AND trên tất cả các hàng, một bit vẫn còn trong mặt nạ cuối cùng khi và chỉ khi mỗi hàng có ít nhất một phần tử chứa bit đó. Đây chính xác là điều kiện bắt buộc để chọn một phần tử trên mỗi hàng sao cho tất cả các phần tử được chọn chia sẻ bit đó, đảm bảo AND cuối cùng là dương. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    
    global_mask = (1 << 60) - 1  # large enough for constraints
    
    for _ in range(n):
        row_mask = 0
        row = list(map(int, input().split()))
        for x in row:
            row_mask |= x
        global_mask &= row_mask
    
    print("YES" if global_mask != 0 else "NO")

if __name__ == "__main__":
    solve()
```Giải pháp đọc từng hàng, nén nó thành một mặt nạ bit duy nhất, sau đó giao nhau tất cả các mặt nạ hàng bằng cách sử dụng AND theo bit. Việc khởi tạo của`global_mask`với tất cả các bit được đặt đảm bảo rằng hàng đầu tiên xác định chính xác tập hợp các bit có thể bắt đầu. 

Một điểm tinh tế là chúng ta không bao giờ cần theo dõi các phần tử đã chọn thực tế. Bước OR trừu tượng hóa mỗi hàng vào không gian bit có sẵn của nó và bước AND thực thi tính nhất quán giữa các hàng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2 3
1 2 4
4 8 1
```Xử lý hàng: 

| Bước | Hàng | Mặt nạ hàng | Mặt nạ toàn cầu | 
| --- | --- | --- | --- | 
| 1 | [1,2,4] | 111 (7) | 111 | 
| 2 | [4,8,1] | 10101 (21) | 101 | 

Mặt nạ chung cuối cùng khác 0, vì vậy câu trả lời là CÓ. 

Điều này xác nhận rằng bit 0 có sẵn ở cả hai hàng, cho phép chọn 1 từ mỗi hàng. 

### Ví dụ 2 

đầu vào:```
2 2
2 4
8 16
```| Bước | Hàng | Mặt nạ hàng | Mặt nạ toàn cầu | 
| --- | --- | --- | --- | 
| 1 | [2,4] | 110 (6) | 110 | 
| 2 | [8,16] | 11000 (24) | 0 | 

Mặt nạ cuối cùng là 0 nên câu trả lời là KHÔNG. 

Điều này thể hiện trường hợp mỗi hàng có các bit hợp lệ bên trong nhưng không có bit nào được chia sẻ trên tất cả các hàng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(NM) | mỗi phần tử được xử lý một lần để tạo mặt nạ hàng | 
| Không gian | O(N) | chỉ yêu cầu bộ nhớ trên mỗi hàng hoặc mặt nạ phát trực tuyến | 

Thuật toán này hiệu quả đối với các ràng buộc điển hình trong đó N và M có tổng số phần tử lên tới 10^5. Các hoạt động bit là thời gian không đổi cho mỗi phần tử, do đó giải pháp phù hợp thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    
    n, m = map(int, sys.stdin.readline().split())
    global_mask = (1 << 60) - 1
    
    for _ in range(n):
        row = list(map(int, sys.stdin.readline().split()))
        row_mask = 0
        for x in row:
            row_mask |= x
        global_mask &= row_mask
    
    return "YES\n" if global_mask != 0 else "NO\n"

# provided samples
assert run("2 3\n1 2 4\n4 8 1\n") == "YES\n"

# all zeros case
assert run("2 2\n0 0\n0 0\n") == "NO\n"

# single row
assert run("1 3\n1 2 4\n") == "YES\n"

# disjoint bits across rows
assert run("2 2\n1 2\n4 8\n") == "NO\n"

# full overlap via one bit
assert run("3 2\n1 0\n1 2\n5 1\n") == "YES\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 2 / 0 0 / 0 0 | KHÔNG | tất cả các số 0 không thể tạo ra AND dương | 
| 1 3 / 1 2 4 | CÓ | một hàng luôn hoạt động nếu bất kỳ số nào dương | 
| 2 2 / 1 2 / 4 8 | KHÔNG | bộ bit rời rạc trên các hàng | 
| 3 2 / 1 0 / 1 2 / 5 1 | CÓ | giao lộ tồn tại thông qua bit 0 | 

## Vỏ cạnh 

Một trường hợp cạnh quan trọng là khi các hàng chỉ chứa số 0. Ví dụ:```
2 2
0 0
0 0
```Mỗi mặt nạ hàng trở thành 0, do đó mặt nạ chung trở thành 0 ngay lập tức. Thuật toán đưa ra NO, điều này đúng vì không có lựa chọn nào có thể tạo ra AND dương. 

Một trường hợp khác là một hàng duy nhất:```
1 3
1 2 4
```Mặt nạ hàng khác 0 và vì không có hàng thứ hai để loại bỏ các bit nên mặt nạ tổng thể vẫn khác 0. Câu trả lời là CÓ vì chúng ta có thể chọn bất kỳ số dương nào và AND của nó trên một phần tử là chính nó. 

Một trường hợp tinh tế hơn là phân phối bit rời rạc:```
2 2
1 2
4 8
```Mặt nạ hàng là 3 và 12 và giao điểm của chúng là 0. Mặc dù mỗi hàng chứa các bit hợp lệ nhưng không có bit nào xuất hiện ở cả hai hàng, do đó không có lựa chọn nào có thể giữ lại một chút trong AND cuối cùng.
