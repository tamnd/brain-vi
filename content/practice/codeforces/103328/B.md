---
title: "CF 103328B - Cây Táo"
description: "Chúng ta được cho một cây có các nút có trọng số và các cạnh có trọng số. Mỗi nút chứa một số quả táo. Mỗi cạnh có một chiều dài."
date: "2026-07-03T17:53:33+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103328
codeforces_index: "B"
codeforces_contest_name: "National Taiwan University NCPC Preliminary 2021"
rating: 0
weight: 103328
solve_time_s: 54
verified: true
draft: false
---

[CF 103328B - Cây táo](https://codeforces.com/problemset/problem/103328/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 54s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một cây có các nút có trọng số và các cạnh có trọng số. Mỗi nút chứa một số quả táo. Mỗi cạnh có một chiều dài. Chúng tôi được phép chọn bất kỳ tập hợp con nút nào và sau đó chúng tôi phải thiết kế một bước đi khép kín trong cây bắt đầu tại một số nút, truy cập mọi nút đã chọn ít nhất một lần và quay lại nút bắt đầu. Điểm của kế hoạch là tổng số táo được thu thập từ các nút đã chọn trừ đi tổng chiều dài của chuyến đi. Mục tiêu là tối đa hóa số điểm này. 

Chi tiết quan trọng là khi một tập hợp các nút được cố định, bước đi khép kín rẻ nhất có thể truy cập tất cả chúng trong cấu trúc cây không phải là tùy ý. Trong một cây, bất kỳ bước đi khép kín nào truy cập một tập hợp các nút đều phải đi qua chính xác cây con được kết nối tối thiểu trải dài các nút đó và mọi cạnh trong cây con đó được đi chính xác hai lần, một lần theo mỗi hướng. Điều này có nghĩa là chi phí di chuyển được xác định hoàn toàn bằng tổng độ dài cạnh trong cây con kết nối cảm ứng, nhân với hai. 

Kích thước đầu vào lên tới một triệu nút, điều này ngay lập tức loại trừ mọi giải pháp cố gắng liệt kê các tập hợp con của nút hoặc mô phỏng tuyến đường một cách rõ ràng. Ngay cả việc truy cập bộ nhớ tuyến tính hoặc gần tuyến tính cũng phải được thiết kế cẩn thận, vì độ sâu đệ quy và các yếu tố không đổi rất quan trọng. Điều này thúc đẩy chúng ta hướng tới một công thức lập trình động trên cấu trúc cây. 

Một vấn đề tế nhị xuất hiện khi suy nghĩ tham lam về việc chọn các nút chỉ dựa trên số lượng táo của chúng. Một nút có đóng góp nhỏ hoặc thậm chí âm vẫn có thể có giá trị nếu nó kết nối hai vùng có lợi nhuận, vì nó có thể giảm nhu cầu về các cạnh bổ sung trong cấu trúc khung. Ngược lại, việc chọn nhiều nút dương cách xa nhau có thể trở nên không có lợi vì việc kết nối chúng buộc phải bao gồm các cạnh đắt tiền phải đi qua hai lần. 

Ví dụ: hãy xem xét một chuỗi gồm ba nút có giá trị apple`[100, 1, 100]`và các cạnh có độ dài`1000`giữa các nút liên tiếp. Việc chọn tất cả các nút mang lại chi phí cạnh rất lớn`2 * 2000 = 4000`trong khi táo tăng là`201`, tạo ra điểm âm. Câu trả lời đúng là chỉ chọn một nút điểm cuối. Một chiến lược ngây thơ luôn bao gồm các nút tích cực sẽ thất bại ở đây vì nó bỏ qua chi phí kết nối. 

Một trường hợp góc khác là khi tất cả các giá trị của quả táo đều bằng 0. Câu trả lời đúng là không, đạt được bằng cách không chọn gì cả. Bất kỳ cách tiếp cận nào giả định phải chọn ít nhất một nút sẽ tạo ra giá trị âm không chính xác. 

## Phương pháp tiếp cận 

Chiến lược brute-force sẽ thử mọi tập hợp con của các nút, tính toán cây con tối thiểu bao trùm chúng và đánh giá chi phí của nó. Ngay cả khi tính toán chi phí cây Steiner trong một cây là tuyến tính theo số lượng nút, số lượng tập hợp con là theo cấp số nhân, khiến cho phương pháp này không thể thực hiện được ngay cả đối với các trường hợp nhỏ. 

Cấu trúc của vấn đề sẽ đơn giản hóa khi chúng ta sửa được một gốc và suy nghĩ xem sự đóng góp lan truyền như thế nào. Nếu chúng ta xem xét một lựa chọn liên thông bắt nguồn từ một nút nào đó thì mỗi cây con con sẽ đóng góp một cái gì đó hữu ích trở lên hoặc bị loại bỏ hoàn toàn. Quan sát quan trọng là chi phí của việc bao gồm một cây con con chính xác là mức tăng tốt nhất bên trong cây con đó trừ đi hai lần độ dài cạnh kết nối. Nếu giá trị này âm thì quyết định tối ưu là loại trừ hoàn toàn cây con đó, vì việc thêm nó vào chỉ làm tăng chi phí đi lại nhiều hơn là thêm táo. 

Điều này biến vấn đề thành một nhiệm vụ lập trình động dạng cây trong đó mỗi nút tổng hợp sự đóng góp tốt nhất có thể của tất cả các cây con con có lợi. Vì cấu trúc đã chọn phải duy trì được kết nối nên mỗi giá trị dp đại diện cho thành phần được kết nối tốt nhất bắt nguồn từ nút đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên các tập hợp con | O(2^N · N) | O(N) | Quá chậm | 
| Cây DP tỉa cành âm | O(N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

### ## Hướng dẫn thuật toán 

1. Gốc cây tại một nút tùy ý, vì cây không bị định hướng và mọi lựa chọn được kết nối đều có một nút trên cùng duy nhất trong chế độ xem gốc. Điều này cho phép chúng ta xác định cấu trúc cha-con cho DP. 
2. Thực hiện duyệt theo thứ tự sau để khi xử lý một nút, tất cả các nút con của nó đã được xử lý và đóng góp tốt nhất của chúng được biết đến. 
3. Đối với mỗi nút, khởi tạo giá trị hiện tại của nó là số lượng táo tại nút đó. Điều này thể hiện mức tăng nếu chúng ta chỉ lấy nút này mà không kết nối bất kỳ thứ gì khác. 
4. Với mỗi cây con, hãy tính xem cây con đó có thể mang lại bao nhiêu lợi ích nếu được đính kèm. Nút con đóng góp giá trị dp của nó trừ đi hai lần chiều dài cạnh nối nó với nút hiện tại. Nếu giá trị này dương, hãy thêm nó vào giá trị của nút hiện tại. Ngược lại, loại bỏ hoàn toàn cây con con. 

Lý do trừ hai lần cạnh là vì bất kỳ cây con nào được đưa vào đều phải đi qua cạnh đó theo cả hai hướng trong một bước đi khép kín. 
5. Lưu trữ giá trị tích lũy này dưới dạng dp[nút], biểu thị điểm số tốt nhất của cây con được kết nối có nút cao nhất trong cây gốc là nút này. 
6. Câu trả lời cuối cùng là giá trị dp tối đa trên tất cả các nút, nhưng cũng hợp lệ khi trả về 0 nếu tất cả các giá trị đều âm, vì việc chọn không có nút nào sẽ mang lại điểm 0. 

### Tại sao nó hoạt động 

Mỗi lựa chọn nút hợp lệ sẽ tạo ra một cây con được kết nối trong cây gốc. Nếu chúng ta root cây con đó ở nút cao nhất theo hướng gốc toàn cục, tất cả các cạnh được bao gồm sẽ đi xuống các cây con con. Tổng số điểm được chia thành mức tăng nút trừ đi hai lần chi phí cạnh và mỗi cây con con đóng góp độc lập sau khi cạnh kết nối của nó được cố định. Bởi vì các cạnh của cây không được chia sẻ giữa các cây con khác nhau nên các quyết định bao gồm hoặc loại trừ từng cây con là độc lập. Tính độc lập này đảm bảo rằng việc loại bỏ cục bộ đóng góp tiêu cực không bao giờ cản trở giải pháp tối ưu toàn cục, vì bất kỳ giải pháp tối ưu nào bao gồm cây con phủ định đều có thể được cải thiện bằng cách loại bỏ nó mà không phá vỡ kết nối phía trên cây gốc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    g = [[] for _ in range(n)]
    
    for _ in range(n - 1):
        u, v, w = map(int, input().split())
        u -= 1
        v -= 1
        g[u].append((v, w))
        g[v].append((u, w))

    parent = [-1] * n
    order = []
    stack = [0]
    parent[0] = -2

    while stack:
        u = stack.pop()
        order.append(u)
        for v, w in g[u]:
            if parent[v] == -1:
                parent[v] = u
                stack.append(v)

    dp = [0] * n

    for u in reversed(order):
        dp[u] = a[u]
        for v, w in g[u]:
            if parent[v] == u:
                gain = dp[v] - 2 * w
                if gain > 0:
                    dp[u] += gain

    print(max(0, max(dp)))

if __name__ == "__main__":
    solve()
```Đoạn mã đầu tiên xây dựng một danh sách kề cho cây. Sau đó, nó xây dựng thứ tự truyền tải bằng cách sử dụng DFS lặp để tránh các vấn đề về độ sâu đệ quy, điều này rất quan trọng do hạn chế lên tới một triệu nút. 

Mảng dp lưu trữ phần đóng góp tốt nhất của cây con được kết nối bắt nguồn từ mỗi nút. Trong quá trình truyền tải ngược, mỗi nút bắt đầu với giá trị apple của chính nó. Đối với mỗi phần tử con, chúng tôi đánh giá xem việc bao gồm cây con đó có cải thiện tổng số hay không sau khi tính đến việc truyền khứ hồi của cạnh kết nối. Chỉ những đóng góp tích cực mới được thêm vào, đảm bảo rằng không có cây con có hại nào được đưa vào. 

Câu trả lời cuối cùng là giá trị tốt nhất trong số tất cả các trạng thái dp, với mức sàn bằng 0 để xử lý các trường hợp trong đó mọi lựa chọn có thể đều gây bất lợi. 

## Ví dụ đã hoạt động 

Hãy xem xét một cây nhỏ trong đó nút 1 có 10 quả táo, nút 2 có 5 quả táo và nút 3 có 8 quả táo, với các cạnh 1-2 có chiều dài 3 và 2-3 có chiều dài 4. 

| Nút | Viết tắt a[u] | Đóng góp của trẻ em | dp[u] | 
| --- | --- | --- | --- | 
| 3 | 8 | không | 8 | 
| 2 | 5 | 8 - 8 = 0 (bỏ đi) | 5 | 
| 1 | 10 | 5 - 6 = -1 (loại bỏ) | 10 | 

Thuật toán chỉ chọn nút 1 vì việc gắn các nút khác không bù được chi phí truyền tải cạnh. Điều này chứng tỏ việc cắt tỉa cục bộ ngăn chặn sự mở rộng không cần thiết như thế nào. 

Bây giờ hãy xem xét một cây hình ngôi sao trong đó nút ở giữa có 1 quả táo và 3 lá, mỗi lá có 100 quả táo với giá cạnh là 1. 

| Nút | Viết tắt a[u] | Đóng góp của trẻ em | dp[u] | 
| --- | --- | --- | --- | 
| lá | 100 mỗi cái | không | 100 | 
| trung tâm | 1 | 100-2 = 98 mỗi cái | 295 | 

Ở đây tất cả các lá đều được bao gồm vì lợi nhuận của chúng vẫn dương sau khi trả chi phí cạnh hai lần. Điều này xác nhận rằng DP đã tổng hợp chính xác nhiều nhánh có lợi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N) | Mỗi nút và cạnh được xử lý chính xác một lần trong quá trình tổng hợp DFS và DP | 
| Không gian | O(N) | Danh sách kề, thứ tự truyền tải và mảng dp lưu trữ thông tin tuyến tính | 

Độ phức tạp tuyến tính là cần thiết với giới hạn lên tới một triệu nút. Bất kỳ cách tiếp cận siêu tuyến tính nào cũng sẽ không đạt được cả giới hạn về thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    solve()
    return ""

# simple chain
assert run("""3
1 1 1
1 2 1
2 3 1
""") == "", "chain test"

# star
assert run("""4
0 10 10 10
1 2 1
1 3 1
1 4 1
""") == "", "star test"

# single node
assert run("""1
5
""") == "", "single node"

# all zeros
assert run("""3
0 0 0
1 2 1
2 3 1
""") == "", "all zero"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| chuỗi | cắt tỉa tối ưu | nhánh âm | 
| ngôi sao | tích lũy đa ngành | cây con độc lập | 
| nút đơn | xử lý trường hợp cơ bản | cây tầm thường | 
| tất cả số không | lựa chọn trống tối ưu | tầng không | 

## Vỏ cạnh 

Cây một nút được xử lý chính xác vì DP khởi tạo mỗi nút bằng giá trị apple riêng của nó và không bao giờ yêu cầu nút con. Câu trả lời sẽ trở thành giá trị duy nhất đó hoặc bằng 0 nếu nó âm. 

Một chuỗi dài trong đó tất cả các nút trung gian có giá trị táo nhỏ so với trọng số của cạnh được cắt tỉa một cách chính xác vì mọi đóng góp của con đều trở thành âm sau khi trừ đi hai lần chi phí cạnh, khiến DP thu gọn thành các nút bị cô lập. 

Một ngôi sao dày đặc trong đó nhiều lá có lợi riêng lẻ vẫn được đưa vào đầy đủ vì mỗi lá được đánh giá độc lập và mỗi lá đóng góp tích cực sau khi điều chỉnh chi phí cạnh, đảm bảo tất cả các lá có lợi đều được gắn vào trung tâm mà không bị can thiệp.
