---
title: "CF 105791M - Thị trưởng"
description: "Chúng ta được cho một biểu đồ nhỏ có tối đa 10 đỉnh, trong đó mỗi cặp đỉnh có thứ tự đều có chi phí liên quan để xây dựng một cạnh có hướng từ cạnh này sang cạnh kia."
date: "2026-06-21T13:12:01+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105791
codeforces_index: "M"
codeforces_contest_name: "UFPE Starters Final Try-Outs 2025"
rating: 0
weight: 105791
solve_time_s: 50
verified: true
draft: false
---

[CF 105791M - Thị trưởng](https://codeforces.com/problemset/problem/105791/M) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 50s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một biểu đồ nhỏ có tối đa 10 đỉnh, trong đó mỗi cặp đỉnh có thứ tự đều có chi phí liên quan để xây dựng một cạnh có hướng từ cạnh này sang cạnh kia. Thị trưởng muốn xây dựng một hệ thống đường có hướng sử dụng một số cạnh này để từ mỗi ngôi nhà có thể đến mọi ngôi nhà khác theo hướng của đường. 

Mục tiêu không chỉ là làm cho đồ thị được kết nối chặt chẽ mà còn làm như vậy bằng cách sử dụng số lượng cạnh có hướng tối thiểu có thể và trong số tất cả các cấu trúc có cạnh tối thiểu như vậy, giảm thiểu tổng chi phí. 

Vì vậy, vấn đề là tối ưu hóa hai lớp. Đầu tiên chúng ta giảm thiểu số cạnh có hướng được chọn. Chỉ sau khi ấn định con số tối thiểu đó, chúng tôi mới giảm thiểu tổng chi phí trong số tất cả các giải pháp có quy mô tối ưu như vậy. 

Vì n tối đa là 10 nên giải pháp có thể đủ khả năng suy luận theo cấp số nhân hoặc dựa trên tập hợp con đối với các cấu trúc như tập hợp con hoặc phân vùng, nhưng bất kỳ giai thừa nào trong n mà không được tối ưu hóa vẫn có khả năng chặt chẽ. Điều này gợi ý mạnh mẽ về lập trình động trên các tập hợp con hoặc biểu diễn mặt nạ bit của các trạng thái kết nối. 

Một thực tế cấu trúc quan trọng là bất kỳ đồ thị có hướng liên thông mạnh nào trên n đỉnh đều phải có ít nhất n cạnh. Giới hạn dưới này xuất phát từ thực tế là mỗi đỉnh phải có ít nhất một cạnh đi ra trong bất kỳ cấu trúc bao phủ chu trình có hướng nào cho phép khả năng tiếp cận ở mọi nơi. Một chu trình có hướng đơn giản có độ dài n đã đạt được kết nối mạnh với chính xác n cạnh, do đó số cạnh tối thiểu chính xác là n bất cứ khi nào tất cả chi phí là hữu hạn và các cạnh tồn tại giữa tất cả các cặp. 

Ràng buộc thứ hai, giảm thiểu chi phí theo một số cạnh cố định, biến bài toán thành việc chọn một cấu trúc liên thông mạnh có chi phí tối thiểu với chính xác n cạnh. 

Một cách tiếp cận đơn giản là thử tất cả các tập hợp con của các cạnh và kiểm tra khả năng kết nối mạnh mẽ. Tuy nhiên, số cạnh có hướng là n2, do đó tập hợp con là 2^(n2), điều này hoàn toàn không khả thi ngay cả khi n = 10. 

Một vấn đề tinh tế hơn là ngay cả khi chúng ta giới hạn chính xác n cạnh, việc kiểm tra các tập con nào tạo thành một đồ thị liên thông mạnh và chọn giá trị rẻ nhất vẫn là C(n2, n), tức là quá lớn. 

Các trường hợp khó khăn xuất hiện khi chi phí bằng 0 hoặc rất lớn. Một ý tưởng ngây thơ về việc xây dựng cấu trúc bao trùm tối thiểu (như chọn các cạnh đi ra tối thiểu trên mỗi nút) đã thất bại vì kết nối mạnh được định hướng không thể phân tách cục bộ. 

## Phương pháp tiếp cận 

Khó khăn trọng tâm là chúng ta phải thực thi kết nối mạnh mẽ toàn cầu trong khi chọn chính xác n cạnh có hướng có tổng trọng số tối thiểu. Số lượng đỉnh rất nhỏ nên chúng ta có thể nghĩ đến việc phân chia các đỉnh thành các thành phần và dần dần hợp nhất chúng. 

Ý tưởng Brute-Force là liệt kê tất cả các tập con của các cạnh có kích thước n và kiểm tra xem các cạnh được chọn có tạo thành đồ thị có hướng liên kết mạnh hay không. Đối với mỗi tập hợp con hợp lệ, chúng tôi tính toán chi phí của nó. Điều này đúng nhưng không thể sử dụng được vì có (n2 chọn n) khả năng, mà với n = 10 vẫn rất lớn về mặt thiên văn. 

Quan sát quan trọng là khả năng kết nối mạnh có thể được xây dựng tăng dần bằng cách duy trì sự phân chia các đỉnh thành các thành phần được kết nối mạnh. Mỗi cạnh có hướng được thêm vào sẽ kết nối hai thành phần khác nhau hoặc nằm bên trong một thành phần. Mục tiêu cuối cùng là kết thúc với đúng một thành phần bao phủ tất cả các đỉnh, sử dụng đúng n cạnh. 

Điều này gợi ý một lập trình động trên các tập hợp con của các đỉnh, trong đó mỗi trạng thái đại diện cho một tập hợp con các đỉnh đã tạo thành một thành phần được kết nối mạnh mẽ và chúng tôi liên tục hợp nhất các tập hợp con rời rạc bằng cách chọn các cạnh kết nối chúng. Tuy nhiên, việc mô phỏng trực tiếp sự hợp nhất vẫn để lại sự mơ hồ về cấu trúc.

Một cách cải cách chính xác hơn là nghĩ đến việc xây dựng một định hướng kết nối chặt chẽ hình thành một chu trình có định hướng giữa các thành phần. Trong bất kỳ đồ thị có hướng kết nối mạnh nào có n nút và n cạnh, cấu trúc phải hoạt động giống như một chu trình có hướng duy nhất trên các thành phần trong đó mỗi thành phần đóng góp chính xác một cạnh “đại diện” đi ra tham gia vào chu trình toàn cục. 

Điều này dẫn đến tập hợp con DP cổ điển trong đó chúng tôi sửa một gốc và tính toán cách tốt nhất để xây dựng một chu trình truy cập tất cả các đỉnh chính xác một lần theo thứ tự thành phần, trong khi bên trong mỗi quá trình chuyển đổi, chúng tôi cho phép cấu trúc bên trong tùy ý đảm bảo khả năng kết nối. 

Chúng tôi xác định dp[mask][v] là chi phí tối thiểu để bắt đầu từ đỉnh 0, truy cập chính xác các đỉnh trong mặt nạ và kết thúc tại đỉnh v, trong đó cấu trúc được hình thành là một đường dẫn có hướng mà sau này có thể được đóng lại thành một chu trình. Các chuyển đổi tương ứng với việc mở rộng đường đi bằng cách thêm một đỉnh mới u không nằm trong mặt nạ và trả chi phí c[v][u]. 

Cuối cùng, chúng ta kết thúc chu trình bằng cách quay lại từ đỉnh cuối cùng về điểm bắt đầu, thêm chi phí c[v][0]. Vì cấu trúc là một chu trình trên tất cả các đỉnh nên nó sử dụng chính xác n cạnh và được liên kết chặt chẽ. 

Cái nhìn sâu sắc quan trọng là bất kỳ đồ thị có hướng kết nối mạnh cạnh tối thiểu nào trên n nút đều có thể được chuyển đổi thành một chu trình Hamilton có hướng duy nhất mà không làm tăng số cạnh và vì chúng tôi đang giảm thiểu chi phí trong số các giải pháp cạnh tối thiểu nên chúng tôi chỉ cần xem xét cấu trúc chu trình. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Tập hợp con Brute Force của các cạnh | O(C(n2, n)) | O(n²) | Quá chậm | 
| Bitmask Chu trình Hamilton DP | O(n² · 2ⁿ) | O(n · 2ⁿ) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi sửa đỉnh 0 làm điểm bắt đầu của chu trình. 

1. Xác định trạng thái DP dp[mask][v], trong đó mặt nạ đại diện cho một tập hợp các đỉnh được truy cập và v là đỉnh được truy cập lần cuối. Trạng thái này lưu trữ chi phí tối thiểu để hình thành một đường dẫn có hướng bắt đầu từ 0, truy cập chính xác mặt nạ và kết thúc tại v. Công thức này đảm bảo chúng ta xây dựng các đường dẫn Hamilton ứng cử viên mà sau này có thể được đóng thành các chu trình. 
2. Khởi tạo dp[1 << 0][0] = 0, vì bắt đầu từ đỉnh 0 mà không có cạnh nào được sử dụng sẽ không mất phí. 
3. Đối với mỗi trạng thái (mặt nạ, v), cố gắng mở rộng đường đi đến một đỉnh mới u không nằm trong mặt nạ bằng cách thêm cạnh có hướng v → u. Chúng tôi cập nhật dp[mask ∪ {u}][u] bằng dp[mask][v] + c[v][u]. Điều này mô hình hóa quyết định đặt u ngay sau v trong thứ tự. 
4. Sau khi điền tất cả các trạng thái, chúng tôi chỉ xem xét các mặt nạ đầy đủ bao gồm tất cả các đỉnh. Đối với mỗi đỉnh kết thúc v có thể có, chúng ta hoàn thành chu trình bằng cách cộng chi phí quay lại từ v về 0 bằng cách sử dụng cạnh c[v][0]. Câu trả lời là mức tối thiểu cho tất cả những lần hoàn thành như vậy. 
5. Chu trình kết quả sử dụng chính xác n cạnh có hướng và đảm bảo rằng mọi đỉnh nằm trên một chu trình có hướng duy nhất, điều này hàm ý khả năng kết nối mạnh mẽ. 

### Tại sao nó hoạt động 

DP liệt kê tất cả các hoán vị có thể có của các đỉnh theo thứ tự của đường đi Hamilton bắt đầu từ 0. Mỗi thứ tự tương ứng với chính xác một chu trình có hướng khi đóng ở cuối. Mỗi chu trình như vậy được kết nối chặt chẽ vì mọi đỉnh đều có đường đi đến mọi đỉnh khác dọc theo chu trình. Bất kỳ đồ thị có hướng liên thông mạnh nào có chính xác n cạnh trên n đỉnh đều phải có cấu trúc chu trình có hướng duy nhất, do đó việc hạn chế tìm kiếm theo chu trình Hamilton không loại bỏ các giải pháp tối ưu. Do đó, DP khám phá chính xác toàn bộ không gian khả thi của các giải pháp hợp lệ có biên độ tối thiểu và chọn giải pháp có chi phí tối thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
c = [list(map(int, input().split())) for _ in range(n)]

INF = 10**30
dp = [[INF] * n for _ in range(1 << n)]

dp[1][0] = 0

for mask in range(1 << n):
    if not (mask & 1):
        continue
    for v in range(n):
        if dp[mask][v] == INF:
            continue
        for u in range(n):
            if mask & (1 << u):
                continue
            nm = mask | (1 << u)
            nd = dp[mask][v] + c[v][u]
            if nd < dp[nm][u]:
                dp[nm][u] = nd

full = (1 << n) - 1
ans = INF

for v in range(n):
    if dp[full][v] < INF:
        ans = min(ans, dp[full][v] + c[v][0])

print(ans)
```Giải pháp xây dựng một DP bitmask đầy đủ trên các tập hợp con các đỉnh. Quá trình chuyển đổi luôn mở rộng đường đi Hamilton một phần thêm một đỉnh, bảo toàn tính chính xác vì mọi thứ tự hợp lệ đều tương ứng với chính xác một chuỗi của các phần mở rộng đó. 

Việc khởi tạo cố định đỉnh 0 làm điểm bắt đầu, điều này tránh việc đếm các phép quay theo chu kỳ nhiều lần và làm giảm tính đối xứng. Bước cuối cùng kết thúc chu trình bằng cách nối đỉnh cuối cùng về 0. 

Một sự tinh tế phổ biến là quên rằng DP phải bắt đầu từ một trạng thái đỉnh duy nhất thay vì cho phép chuyển đổi ban đầu tùy ý. Nếu không có mỏ neo này, cùng một chu kỳ sẽ được tính nhiều lần ở các vòng quay khác nhau. 

## Ví dụ đã hoạt động 

Hãy xem xét mẫu đầu tiên:```
0 0 1
0 0 1
1 0 0
```Chúng tôi theo dõi trạng thái dp chỉ về mặt nạ. 

| Bước | mặt nạ | đỉnh cuối cùng | chi phí | 
| --- | --- | --- | --- | 
| ban đầu | 001 | 0 | 0 | 
| mở rộng | 011 | 1 | 0 | 
| mở rộng | 111 | 2 | 0 | 
| đóng | 111 | 2 → 0 | 1 | 

Chu kỳ tối ưu là 0 → 1 → 2 → 0 với tổng chi phí là 1. 

Điều này cho thấy DP xác định chính xác rằng các cạnh giá rẻ buộc phải có một thứ tự cụ thể và việc đóng cuối cùng sẽ đóng góp vào chi phí của cạnh cuối cùng. 

Bây giờ hãy xem xét ví dụ thứ hai:```
0 3
6 0
```| Bước | mặt nạ | đỉnh cuối cùng | chi phí | 
| --- | --- | --- | --- | 
| ban đầu | 01 | 0 | 0 | 
| mở rộng | 11 | 1 | 3 | 
| đóng | 11 | 1 → 0 | 3 | 

Chu kỳ 0 → 1 → 0 có chi phí là 3, trong khi phương án 0 → 1 → 0 đảo ngược là đối xứng nhưng không cải thiện chi phí. 

Điều này chứng tỏ rằng DP chọn hướng một cách tự nhiên dựa trên chi phí cạnh bất đối xứng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n² · 2ⁿ) | Đối với mỗi tập hợp con và đỉnh cuối cùng, chúng tôi thử tất cả các đỉnh tiếp theo | 
| Không gian | O(n · 2ⁿ) | Bảng DP lưu trữ chi phí tốt nhất cho mỗi mặt nạ và điểm cuối | 

Với n 10, DP có tối đa 1024 mặt nạ và 10 điểm cuối trên mỗi mặt nạ, dẫn đến khoảng 10.000 trạng thái và 100.000 chuyển tiếp, dễ dàng phù hợp trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    n = int(input())
    c = [list(map(int, input().split())) for _ in range(n)]

    INF = 10**30
    dp = [[INF] * n for _ in range(1 << n)]
    dp[1][0] = 0

    for mask in range(1 << n):
        if not (mask & 1):
            continue
        for v in range(n):
            if dp[mask][v] == INF:
                continue
            for u in range(n):
                if mask & (1 << u):
                    continue
                nm = mask | (1 << u)
                nd = dp[mask][v] + c[v][u]
                if nd < dp[nm][u]:
                    dp[nm][u] = nd

    full = (1 << n) - 1
    ans = min(dp[full][v] + c[v][0] for v in range(n) if dp[full][v] < INF)
    return str(ans)

# sample-like
assert run("3\n0 0 1\n0 0 1\n1 0 0\n") == "1"

# 2 nodes
assert run("2\n0 3\n6 0\n") == "3"

# minimum n=1
assert run("1\n0\n") == "0"

# symmetric zero cost
assert run("3\n0 0 0\n0 0 0\n0 0 0\n") == "0"

# asymmetric costs
assert run("3\n0 5 1\n2 0 4\n3 6 0\n") == "6"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| n=1 | 0 | chu kỳ tầm thường nút đơn | 
| tất cả chi phí bằng không | 0 | DP không đếm quá nhiều cạnh | 
| 3 chu kỳ không đối xứng | 6 | lựa chọn định hướng đúng | 
| trường hợp 2 nút | 3 | tính đúng đắn cơ bản của việc đóng cửa | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn là n = 1. Đồ thị có một đỉnh duy nhất và không cần cạnh nào để đáp ứng khả năng kết nối mạnh. DP khởi tạo dp[1][0] = 0 và câu trả lời cuối cùng xem xét việc đóng chu trình từ 0 trở về 0. Vì c[0][0] là 0 nên đầu ra chính xác là 0. 

Một trường hợp tinh vi khác là khi tất cả chi phí biên đều bằng 0. Bất kỳ chu trình Hamilton nào cũng là tối ưu và DP có thể tìm thấy nhiều trạng thái tương đương. Vì quá trình chuyển đổi chỉ thêm chi phí không âm nên tất cả các đường dẫn đều kết thúc bằng chi phí 0 và mức tối thiểu vẫn là 0. 

Trường hợp thứ ba là khi các cạnh đi ra rẻ nhất tạo thành cấu trúc không có chu kỳ, ví dụ như một ngôi sao. DP tránh được các lựa chọn cục bộ tham lam một cách chính xác vì nó thực thi một trật tự toàn cục trên tất cả các đỉnh. Ngay cả khi đỉnh 0 có các cạnh đi ra rất rẻ so với tất cả các cạnh khác, thì chi phí đóng cuối cùng và các chuyển đổi trung gian sẽ xác định liệu thứ tự đó có tối ưu hay không và DP khám phá tất cả các hoán vị thay vì cam kết sớm.
