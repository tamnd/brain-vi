---
title: "CF 103800E - Màu gừng"
description: "Chúng ta được hoán vị các số từ 1 đến n. Bạn có thể coi nó như một đồ thị có hướng trong đó mỗi nút trỏ đến chính xác một nút khác và vì nó là một hoán vị nên mỗi nút cũng có chính xác một cạnh tới. Cấu trúc này chia thành các chu kỳ có định hướng rời rạc."
date: "2026-07-02T08:43:01+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103800
codeforces_index: "E"
codeforces_contest_name: "The 2022 SDUT Summer Trials"
rating: 0
weight: 103800
solve_time_s: 49
verified: true
draft: false
---

[CF 103800E - Màu của gừng](https://codeforces.com/problemset/problem/103800/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 49s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được hoán vị các số từ 1 đến n. Bạn có thể coi nó như một đồ thị có hướng trong đó mỗi nút trỏ đến chính xác một nút khác và vì nó là một hoán vị nên mỗi nút cũng có chính xác một cạnh tới. Cấu trúc này chia thành các chu kỳ có định hướng rời rạc. 

Mỗi số từ 1 đến n phải được tô màu đen hoặc trắng. Hạn chế ràng buộc mọi vị trí i với giá trị p[i]: màu của i phải khác với màu của p[i]. Nói cách khác, mọi cạnh có hướng i → p[i] đều cấm cả hai điểm cuối chia sẻ cùng một màu. 

Nhiệm vụ là đếm xem có bao nhiêu màu hợp lệ tồn tại dưới ràng buộc này, modulo 998244353. 

Ràng buộc n 100000 buộc chúng ta tránh xa bất kỳ số mũ nào trong n. Một cách tiếp cận ngây thơ thử tất cả các màu 2^n là không thể ngay lập tức vì 2^100000 vượt xa mọi tính toán khả thi. Ngay cả bất kỳ phương pháp nào phân nhánh trên mỗi nút mà không cắt tỉa mạnh cũng sẽ thất bại. Cấu trúc hoán vị gợi ý chúng ta nên khai thác sự phân rã chu trình theo thời gian tuyến tính. 

Trường hợp cạnh tinh tế xuất hiện khi một nút trỏ đến chính nó. Nếu p[i] = i, thì điều kiện buộc color(i) ≠ color(i), điều này là không thể. Vì vậy, bất kỳ điểm cố định nào cũng ngay lập tức đưa ra câu trả lời bằng 0. Tổng quát hơn, bất kỳ chu kỳ lẻ nào cũng sẽ trở thành không thể. 

Ví dụ: nếu n = 1 và p = [1], nút duy nhất phải khác với chính nó, điều này là không thể, vì vậy câu trả lời là 0. 

Nếu n = 2 và p = [2, 1], chúng ta nhận được một chu kỳ có độ dài 2, hợp lệ và mang lại 2 màu. 

Nếu n = 3 và p = [2, 3, 1], chu trình có độ dài 3 và tính nhất quán sẽ bị phá vỡ khi chúng ta quay lại từ đầu, khiến câu trả lời là 0. 

## Phương pháp tiếp cận 

Ý tưởng brute-force là gán cho mỗi phần tử trong số n phần tử màu đen hoặc trắng và sau đó kiểm tra tất cả các ràng buộc. Điều này liệt kê 2^n phép gán và mỗi lần kiểm tra sẽ quét tất cả các vị trí để xác minh rằng color[i] khác với color[p[i]]. Chi phí là O(n · 2^n), quá lớn ngay cả với n = 30, vì nó đã vượt quá một tỷ phép tính. 

Quan sát quan trọng xuất phát từ cấu trúc của một hoán vị: mọi phần tử đều thuộc về đúng một chu trình. Bên trong chu trình v1 → v2 → ... → vk → v1, ràng buộc buộc các phần tử liền kề trong chu trình này phải có màu xen kẽ. Điều này biến mỗi chu kỳ thành một vấn đề nhất quán về tính chẵn lẻ. 

Nếu chúng ta cố gắng gán màu cho v1 thì phần còn lại của chu kỳ sẽ bị ép buộc: v2 phải khác v1, v3 phải khác v2, v.v. Khi quay lại vk, chúng ta phải đảm bảo nó cũng khác với v1. Điều này chỉ hoạt động khi độ dài chu kỳ là chẵn, vì sự luân phiên trở về màu ban đầu chính xác khi số bước là chẵn. Đối với các chu kỳ lẻ, ta có mâu thuẫn. 

Đối với mỗi chu kỳ có độ dài chẵn, có chính xác hai phép gán hợp lệ: chúng ta có thể chọn nút bắt đầu là đen hoặc trắng và phần còn lại là bắt buộc. Các chu kỳ khác nhau là độc lập nên tổng số màu là 2 được nâng lên bằng số chu kỳ chẵn, miễn là không có chu kỳ lẻ nào cả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n · 2^n) | O(n) | Quá chậm | 
| Phân hủy chu kỳ | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi giải quyết vấn đề bằng cách phân tách hoán vị thành các chu trình và phân tích từng chu trình một cách độc lập. 

### bước 

1. Đánh dấu tất cả các nút là chưa được truy cập và chuẩn bị bộ đếm chu kỳ và cờ cho biết câu trả lời đã hợp lệ hay chưa. 
2. Lặp lại từng nút i từ 1 đến n. Nếu tôi đã được truy cập rồi, hãy bỏ qua vì nó thuộc về một chu trình đã được phát hiện trước đó. 
3. Nếu i chưa được thăm, hãy bắt đầu đi dọc theo các cạnh hoán vị i → p[i] nhiều lần cho đến khi quay lại nút đã thăm. Đánh dấu mọi nút đã truy cập trong quá trình đi bộ này và đếm xem có bao nhiêu nút trong chu kỳ này.

Lý do điều này có tác dụng là vì một hoán vị đảm bảo mỗi nút thuộc về chính xác một chu kỳ, do đó việc truyền tải này sẽ khôi phục chính xác một thành phần được kết nối. 
4. Sau khi trích xuất một chu trình, hãy kiểm tra độ dài của nó. Nếu độ dài là số lẻ, ngay lập tức đánh dấu câu trả lời là 0 và ngừng suy luận thêm về các chu kỳ khác, vì một mâu thuẫn duy nhất sẽ làm mất hiệu lực toàn bộ màu sắc. 
5. Nếu độ dài chu kỳ là chẵn, hãy tăng số lượng chu kỳ hợp lệ. 
6. Sau khi xử lý tất cả các nút, tính kết quả cuối cùng là 2^(số chu kỳ chẵn) modulo 998244353. 

### Tại sao nó hoạt động 

Trong mỗi chu kỳ, ràng buộc c[i] ≠ c[p[i]] buộc các màu sắc phải xen kẽ dọc theo các cạnh của chu kỳ. Điều này có nghĩa là khi màu của bất kỳ nút nào được chọn, tất cả các nút khác trong chu trình sẽ được xác định duy nhất. Việc quay lại nút bắt đầu đặt ra yêu cầu nhất quán chỉ phụ thuộc vào tính chẵn lẻ của độ dài chu kỳ. Chu kỳ chẵn bảo toàn tính nhất quán, chu kỳ lẻ mâu thuẫn với nó. Vì các chu kỳ rời rạc nên các lựa chọn của chúng nhân lên một cách độc lập, tạo ra sản phẩm gồm 2 lựa chọn cho mỗi chu kỳ chẵn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

n = int(input())
p = [0] + list(map(int, input().split()))

visited = [False] * (n + 1)
even_cycles = 0

for i in range(1, n + 1):
    if visited[i]:
        continue

    cur = i
    cycle_len = 0

    while not visited[cur]:
        visited[cur] = True
        cycle_len += 1
        cur = p[cur]

    if cycle_len % 2 == 1:
        print(0)
        sys.exit(0)

    even_cycles += 1

print(pow(2, even_cycles, MOD))
```Việc truyền tải sử dụng bước đi theo chu kỳ tiêu chuẩn trên đồ thị hàm số được tạo ra bởi hoán vị. Mỗi nút được đánh dấu là đã truy cập vào lần đầu tiên nó được gặp, do đó mỗi chu kỳ được xử lý chính xác một lần. 

Kiểm tra tính chẵn lẻ về độ dài chu kỳ là bước quan trọng: bất kỳ chu kỳ lẻ nào cũng buộc phải chấm dứt ngay lập tức với đầu ra bằng 0. Mặt khác, mỗi chu kỳ chẵn đóng góp hệ số nhân là 2. 

Phép lũy thừa cuối cùng sử dụng sức mạnh mô-đun tích hợp của Python, chạy theo thời gian logarit và an toàn theo mô-đun. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 2
p = [2, 1]
```Phân rã chu trình tạo ra một chu trình đơn: (1 → 2 → 1). 

| Bước | Nút | Kích thước chu kỳ cho đến nay | Đã truy cập | 
| --- | --- | --- | --- | 
| bắt đầu | 1 | 0 | {} | 
| thăm | 1 | 1 | {1} | 
| thăm | 2 | 2 | {1,2} | 
| trở lại | 1 | dừng lại | chu kỳ hoàn thành | 

Độ dài chu kỳ là 2, chẵn nên có 2 màu hợp lệ. Câu trả lời là 2^1 = 2. 

Điều này xác nhận rằng chu kỳ 2 cho phép thực hiện chính xác hai nhiệm vụ luân phiên. 

### Ví dụ 2 

đầu vào:```
n = 3
p = [2, 3, 1]
```Điều này tạo thành một chu kỳ có độ dài 3. 

| Bước | Nút | Kích thước chu kỳ | Đã truy cập | 
| --- | --- | --- | --- | 
| bắt đầu | 1 | 0 | {} | 
| thăm | 1 | 1 | {1} | 
| thăm | 2 | 2 | {1,2} | 
| thăm | 3 | 3 | {1,2,3} | 
| trở lại | 1 | dừng lại | chu kỳ hoàn thành | 

Độ dài chu kỳ là số lẻ nên không tồn tại màu hợp lệ. Câu trả lời là 0. 

Điều này cho thấy sự mâu thuẫn xuất hiện như thế nào khi các màu xen kẽ quấn quanh một chu kỳ có độ dài lẻ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi nút được truy cập chính xác một lần trong khi hình thành chu kỳ và lũy thừa mô-đun là O(log n) | 
| Không gian | O(n) | Đã truy cập mảng và lưu trữ hoán vị | 

Việc truyền tải tuyến tính phù hợp thoải mái trong ràng buộc n ≤ 100000 và thuật toán chỉ thực hiện việc đuổi theo con trỏ đơn giản cộng với một phép lũy thừa, nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 998244353

def solve(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(input())
    p = [0] + list(map(int, input().split()))

    visited = [False] * (n + 1)
    even_cycles = 0

    for i in range(1, n + 1):
        if visited[i]:
            continue
        cur = i
        cycle_len = 0
        while not visited[cur]:
            visited[cur] = True
            cycle_len += 1
            cur = p[cur]
        if cycle_len % 2 == 1:
            return "0"
        even_cycles += 1

    return str(pow(2, even_cycles, MOD))

# sample-like tests
assert solve("2\n2 1\n") == "2"
assert solve("3\n2 3 1\n") == "0"

# minimum size
assert solve("1\n1\n") == "0"

# all fixed points
assert solve("4\n1 2 3 4\n") == "0"

# two disjoint even cycles
assert solve("4\n2 1 4 3\n") == "4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Hoán đổi 2 chu kỳ | 2 | chu kỳ chẵn đơn | 
| 3 chu kỳ | 0 | chu kỳ lẻ không hợp lệ | 
| n=1 tự lặp | 0 | trường hợp cạnh nhỏ nhất | 
| hoán vị danh tính | 0 | nhiều chu kỳ lẻ | 
| hai 2 chu kỳ | 4 | sự độc lập của chu kỳ | 

## Vỏ cạnh 

Một điểm cố định chẳng hạn như p[i] = i ngay lập tức tạo ra một chu kỳ có độ dài 1. Trong quá trình truyền tải, thuật toán phát hiện độ dài chu kỳ là 1 và kết thúc bằng 0. Ví dụ, đầu vào`1 1`tạo ra độ dài chu kỳ 1 và bị từ chối. 

Một hoán vị bao gồm hoàn toàn các bản đồ tự hoặc chu kỳ lẻ sẽ sớm bị phá hủy vì chu kỳ lẻ được phát hiện đầu tiên sẽ kích hoạt sự chấm dứt ngay lập tức. Điều này ngăn việc di chuyển không cần thiết đến phần còn lại của biểu đồ trong khi vẫn duy trì tính chính xác vì bất kỳ chu kỳ không hợp lệ nào cũng khiến toàn bộ cấu hình không thể thực hiện được. 

Một trường hợp có nhiều chu kỳ chẵn, chẳng hạn như`2 1 4 3`, tạo ra hai chu kỳ độc lập có độ dài 2. Mỗi chu kỳ đóng góp hệ số 2 và thuật toán nhân chính xác chúng để thu được 4, phản ánh các lựa chọn nhị phân độc lập trên mỗi chu kỳ.
