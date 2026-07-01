---
title: "CF 104427B - Luật sư"
description: "Chúng tôi được trao một mối quan hệ trực tiếp giữa các luật sư. Mỗi mối quan hệ nói lên rằng một luật sư tin tưởng một luật sư khác và sự tin tưởng này có ý nghĩa hoạt động rất cụ thể: nếu luật sư B tin tưởng luật sư A thì A sẽ bào chữa cho B."
date: "2026-06-30T18:58:18+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104427
codeforces_index: "B"
codeforces_contest_name: "2022-2023 Winter Petrozavodsk Camp, Day 2: GP of ainta"
rating: 0
weight: 104427
solve_time_s: 50
verified: true
draft: false
---

[CF 104427B - Luật sư](https://codeforces.com/problemset/problem/104427/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 50s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được ban cho một mối quan hệ trực tiếp giữa các luật sư. Mỗi mối quan hệ nói lên rằng một luật sư tin tưởng một luật sư khác và sự tin tưởng này có ý nghĩa hoạt động rất cụ thể: nếu luật sư B tin tưởng luật sư A thì A sẽ bào chữa cho B. 

Vì vậy, đầu vào xác định một đồ thị có hướng trong đó cạnh A → B có nghĩa là A bảo vệ B. Một luật sư có thể bào chữa cho nhiều người khác và một luật sư được coi là trắng án nếu có ít nhất một luật sư khác bào chữa cho họ, điều này có nghĩa là có ít nhất một cạnh sắp tới trong đồ thị có hướng này. 

Có một ngoại lệ toàn cầu duy nhất làm thay đổi cấu trúc hiệu lực. Nếu hai luật sư bào chữa cho nhau cùng một lúc, nghĩa là có một chu kỳ có chiều dài bằng hai giữa họ, thì cả hai luật sư đó sẽ tự động bị tuyên bố có tội bất kể họ có thể có bất kỳ biện pháp bào chữa nào khác. 

Nhiệm vụ là xác định liệu mọi luật sư có thể được trắng án theo những quy tắc này hay không, vì tất cả các mối quan hệ tin cậy đều cố định và không thể sửa đổi. 

Các ràng buộc lên tới hai trăm nghìn luật sư và hai trăm nghìn quan hệ. Điều này ngay lập tức ngụ ý rằng bất kỳ giải pháp nào cũng phải tuyến tính hoặc gần tuyến tính theo kích thước của đồ thị. Cách tiếp cận kiểu O(NM) hoặc O(N^2) sẽ vượt xa giới hạn khả thi, trong khi cách tiếp cận O(N + M) hoặc O(M log N) có thể chấp nhận được. 

Một sai lầm ngây thơ ở đây là chỉ tập trung vào điều kiện "ít nhất một phòng thủ sắp đến" và bỏ qua quy tắc phòng thủ chung. Ví dụ: trong biểu đồ như 1 ↔ 2 và 3 ↔ 4, mỗi nút có ít nhất một cạnh đến, nhưng cả hai cặp đều không hợp lệ do bảo vệ lẫn nhau, khiến câu trả lời là KHÔNG. 

Một trường hợp lỗi tinh vi khác là khi một nút không có cạnh nào cả. Ngay cả khi biểu đồ dày đặc, một nút như vậy sẽ khiến cho việc tuyên trắng án phổ biến là không thể. 

## Phương pháp tiếp cận 

Cách giải thích thô bạo trực tiếp sẽ mô phỏng tình trạng của từng luật sư một cách độc lập. Đối với mỗi nút, chúng tôi sẽ quét tất cả các cạnh đến để xác minh rằng có ít nhất một nút tồn tại. Sau đó, chúng tôi sẽ quét tất cả các cặp nút để phát hiện xem có cặp bảo vệ lẫn nhau nào tồn tại hay không và vô hiệu hóa các nút đó. 

Điều này hoạt động về mặt khái niệm vì nó tuân theo định nghĩa một cách chính xác. Tuy nhiên, chi phí quét tất cả các cặp để tìm mối quan hệ tương hỗ là phương trình bậc hai trong trường hợp xấu nhất. Với tối đa 200.000 nút, việc kiểm tra theo cặp sẽ bao hàm sự so sánh lên tới 4 × 10^10, điều này là không khả thi. 

Quan sát quan trọng là cả hai điều kiện đều cục bộ đối với các cạnh. Việc một nút có cạnh đến có thể được theo dõi bằng bộ đếm độ đơn giản hay không. Việc bảo vệ lẫn nhau có tồn tại hay không có thể được kiểm tra trong quá trình xử lý đầu vào bằng cách đánh dấu các cạnh được định hướng trong tập băm hoặc tập kề và xác minh xem cạnh ngược đã tồn tại hay chưa. 

Điều này làm giảm vấn đề từ suy luận tổng thể trên tất cả các cặp đến kiểm tra thời gian không đổi trên mỗi cạnh. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(N2 + M) | O(N + M) | Quá chậm | 
| Tối ưu | O(N + M) | O(N + M) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi mô hình hóa tình huống dưới dạng biểu đồ có hướng trong đó mỗi mối quan hệ tin cậy trở thành một cạnh có hướng. 

### Các bước 

1. Đọc tất cả các cạnh và xây dựng biểu diễn đồ thị có hướng. 

Mỗi cạnh A → B chỉ ra rằng A bảo vệ B. 
2. Duy trì một mảng`indegree`Ở đâu`indegree[v]`đếm xem có bao nhiêu luật sư bào chữa cho luật sư v. 

Mỗi lần đọc cạnh A → B, chúng ta tăng`indegree[B]`. 

Điều này trực tiếp cho biết liệu mỗi luật sư có nhận được ít nhất một lời bào chữa hay không. 
3. Đồng thời, lưu trữ tất cả các cạnh được định hướng trong một bộ. 

Điều này cho phép kiểm tra liên tục xem cạnh ngược có tồn tại hay không. 
4. Đối với mỗi cạnh A → B, kiểm tra xem cạnh ngược B → A có tồn tại hay không. 

Nếu đúng như vậy, chúng ta biết ngay rằng có một cặp bảo vệ lẫn nhau tồn tại, điều này khiến cấu hình không hợp lệ. 
5. Sau khi xử lý tất cả các cạnh, hãy xác minh rằng mọi luật sư đều có ít nhất 1 bằng cấp. 

Nếu bất kỳ luật sư nào có bằng cấp 0 thì họ không thể được trắng án. 
6. Nếu cả hai điều kiện đều được thỏa mãn, xuất ra CÓ. Nếu không thì xuất ra NO. 

### Tại sao nó hoạt động 

Thuật toán mã hóa trực tiếp cả hai điều kiện cần thiết được ngụ ý bởi các quy tắc bài toán. Điều kiện không cấp độ đảm bảo mọi luật sư đều nhận được ít nhất một lần bào chữa. Kiểm tra cạnh nhau thực thi quy tắc rằng bất kỳ cặp phòng thủ tương hỗ nào cũng sẽ tự động vô hiệu hóa cả hai người tham gia. 

Vì cả hai điều kiện chỉ phụ thuộc vào thông tin cạnh cục bộ và mỗi cạnh được kiểm tra chính xác một lần nên không có sự phụ thuộc toàn cục ẩn. Mọi cấu hình hợp lệ đều phải đáp ứng các điều kiện này và mọi vi phạm chúng đều tương ứng trực tiếp với vi phạm quy tắc trong báo cáo vấn đề. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    
    indeg = [0] * (n + 1)
    edges = set()
    
    bad = False
    
    for _ in range(m):
        a, b = map(int, input().split())
        # a defends b => a -> b
        indeg[b] += 1
        edges.add((a, b))
    
    # check mutual defense
    for a, b in edges:
        if (b, a) in edges:
            bad = True
            break
    
    if bad:
        print("NO")
        return
    
    for i in range(1, n + 1):
        if indeg[i] == 0:
            print("NO")
            return
    
    print("YES")

if __name__ == "__main__":
    solve()
```Giải pháp tách biệt hai ràng buộc một cách rõ ràng. Mảng indegree được cập nhật trong vòng lặp đầu vào nên không cần chuyển qua danh sách kề thứ hai cho phần đó. Bộ cạnh được sử dụng hoàn toàn để phát hiện các cặp tương hỗ; sử dụng một bộ đảm bảo thời gian tra cứu trung bình O(1). 

Một chi tiết triển khai tinh tế là các cạnh tương hỗ được kiểm tra sau khi đọc tất cả đầu vào thay vì trong khi chèn. Cả hai cách tiếp cận đều hiệu quả, nhưng quá trình xử lý hậu kỳ giúp logic đơn giản hơn và tránh các trường hợp bị thiếu trong đó cả hai hướng xuất hiện sau trong đầu vào. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 3
1 2
2 3
3 1
```| Bước | Cạnh | Bằng cấp | Kiểm tra lẫn nhau | 
| --- | --- | --- | --- | 
| 1 | 1 → 2 | [0,0,1,0] | không | 
| 2 | 2 → 3 | [0,0,1,1] | không | 
| 3 | 3 → 1 | [1,0,1,1] | không | 

Tất cả các nút đều có ít nhất một bậc và không tồn tại cạnh ngược. Cấu hình hợp lệ nên đầu ra là CÓ. 

### Ví dụ 2 

đầu vào:```
4 6
1 2
1 3
1 4
2 3
2 4
3 4
```| Bước | Cạnh | Bằng cấp | Kiểm tra lẫn nhau | 
| --- | --- | --- | --- | 
| 1 | 1 → 2 | [0,0,1,0,0] | không | 
| ... | ... | ... | ... | 
| cuối cùng | - | nút 1 có bậc 0 | không | 

Nút 1 không bao giờ nhận được bất kỳ sự phòng thủ nào đến. Mặc dù biểu đồ dày đặc, nhưng vi phạm duy nhất này khiến cho việc trắng án hoàn toàn là không thể, vì vậy câu trả lời là KHÔNG. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N + M) | Mỗi cạnh được xử lý một lần để đếm độ và một lần để kiểm tra lẫn nhau | 
| Không gian | O(N + M) | Lưu trữ mảng và tập cạnh theo cấp độ | 

Các ràng buộc cho phép lên tới 200.000 nút và cạnh, do đó độ phức tạp tuyến tính là cần thiết. Giải pháp phù hợp thoải mái trong cả giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided samples
assert run("""3 3
1 2
2 3
3 1
""") == "YES"

assert run("""4 6
1 2
1 3
1 4
2 3
2 4
3 4
""") == "NO"

assert run("""4 4
1 2
2 1
3 4
4 3
""") == "NO"

# custom cases
assert run("""1 0
""") == "NO", "single node cannot be defended"

assert run("""2 1
1 2
""") == "NO", "node 1 has indegree 0"

assert run("""2 2
1 2
2 1
""") == "NO", "mutual defense invalidates both"

assert run("""3 2
1 2
2 3
""") == "NO", "node 1 has indegree 0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| nút đơn | KHÔNG | không thể phòng thủ được | 
| chuỗi bảo hiểm bắt đầu bị thiếu | KHÔNG | trường hợp không độ | 
| cặp đôi | KHÔNG | quy tắc hai chiều | 
| chuỗi một phần | KHÔNG | nhiều ràng buộc với nhau | 

## Vỏ cạnh 

Trường hợp cạnh tối thiểu là khi N = 1 và M = 0. Luật sư duy nhất không nhận được lời bào chữa nào, do đó thuật toán đưa ra NO chính xác do mức độ bằng 0. 

Trong trường hợp cạnh đơn hai nút như 1 → 2, nút 1 có mức độ bằng 0 mặc dù nút 2 được bảo vệ đúng cách. Quá trình quét theo mức độ sẽ phát hiện ra điều này ngay lập tức, tạo ra NO. 

Trong một cặp đối xứng như 1 ↔ 2, cả hai nút đều có ít nhất một mức độ, do đó, một giải pháp đơn giản có thể trả về CÓ một cách không chính xác. Tính năng phát hiện cạnh nhau nắm bắt được điều này và từ chối cấu hình một cách chính xác. 

Trong các đồ thị thưa thớt nơi các cạnh tạo thành chuỗi dài, độ chính xác phụ thuộc hoàn toàn vào việc phát hiện nút đầu tiên trong chuỗi có bậc bằng 0. Thuật toán xử lý việc này một cách tự nhiên vì tích lũy mức độ không phụ thuộc vào cấu trúc bên ngoài các lân cận ngay lập tức.
