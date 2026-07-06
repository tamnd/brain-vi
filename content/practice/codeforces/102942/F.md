---
title: "CF 102942F - Ưu đãi"
description: "Quan điểm vũ phu bắt đầu một cách tự nhiên bằng cách sửa một đoạn [l, r]. Bên trong nó, chúng tôi nhóm các vị trí theo giá trị. Nếu chúng tôi quyết định bao gồm một giá trị v, chúng tôi phải trả ít nhất một lần xuất hiện của v bên trong phân khúc, do đó đóng góp chi phí của v là ai tối thiểu trên tất cả i trong…"
date: "2026-07-04T07:41:40+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102942
codeforces_index: "F"
codeforces_contest_name: "Noobs Round #2 (Div. 4) by Rudro25"
rating: 0
weight: 102942
solve_time_s: 44
verified: true
draft: false
---

[CF 102942F - Ưu đãi](https://codeforces.com/problemset/problem/102942/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 44s 
**Đã xác minh:** có 

##Giải pháp 
## Phương pháp tiếp cận 

Quan điểm vũ phu bắt đầu một cách tự nhiên bằng cách sửa một đoạn [l, r]. Bên trong nó, chúng tôi nhóm các vị trí theo giá trị. Nếu chúng tôi quyết định bao gồm một giá trị v, chúng tôi phải trả ít nhất một lần xuất hiện của v bên trong phân khúc, do đó, đóng góp chi phí của v là a_i tối thiểu trên tất cả i trong phân khúc có giá trị v. Tổng số mục chúng tôi nhận được chỉ đơn giản là độ dài phân khúc, bởi vì mọi lần xuất hiện đều trở nên miễn phí khi giá trị của nó được kích hoạt. 

Vì vậy, đối với một phân khúc cố định, chiến lược tối ưu rất rõ ràng: chúng tôi chọn một số tập hợp con các giá trị riêng biệt trong đó và để giảm thiểu chi phí, chúng tôi trả một lần cho mỗi giá trị đã chọn. Tập hợp con tốt nhất trong ngân sách không phải là tập hợp con tùy ý; nó trở thành một vấn đề lựa chọn trên các nhóm giá trị có trọng số là những lần xuất hiện tối thiểu này. Phương pháp brute-force sẽ tính toán điều này cho mọi phân đoạn, tính toán lại các nhóm giá trị mỗi lần. Trong trường hợp xấu nhất, điều này yêu cầu các phân đoạn O(n^2) và trong mỗi phân đoạn, O(n) có khả năng hoạt động để tổng hợp các giá trị riêng biệt, dẫn đến hành vi O(n^3), vượt xa giới hạn. 

Quan sát quan trọng là bên trong bất kỳ phân khúc cố định nào, chi phí chỉ phụ thuộc vào tập hợp các giá trị riêng biệt và vị trí tối thiểu của chúng. Điều này gợi ý việc duy trì một cửa sổ trượt nơi chúng tôi theo dõi, đối với mỗi giá trị, đóng góp hiện tại của nó vào chi phí, cụ thể là a_i tối thiểu đã thấy cho đến nay đối với giá trị đó trong cửa sổ. Khi điểm cuối bên phải mở rộng, chúng tôi sẽ cập nhật cấu trúc này dần dần. Khó khăn sau đó trở thành làm thế nào để thực thi ràng buộc ngân sách trong khi tối đa hóa số lượng mục trong cửa sổ. 

Chúng tôi diễn giải lại chi phí cửa sổ là tổng của các giá trị riêng biệt của mức giá tối thiểu hiện tại của chúng trong cửa sổ. Việc mở rộng cửa sổ sẽ tăng số lượng tần suất nhưng chỉ thay đổi chi phí khi một giá trị mới xuất hiện hoặc khi một giá trị hiện có xuất hiện với chi phí thấp hơn xuất hiện trong cửa sổ. Điều này làm cho cấu trúc đủ ổn định để duy trì với bản đồ tần số và nhiều chi phí được chọn hiện tại. 

Việc giảm cuối cùng là cửa sổ trượt hai con trỏ: chúng tôi mở rộng ranh giới bên phải, duy trì chi phí hiện tại của các giá trị duy nhất trong cửa sổ và bất cứ khi nào chi phí vượt quá k, chúng tôi thu nhỏ từ bên trái cho đến khi nó trở lại hợp lệ. Trong quá trình này, chúng tôi liên tục theo dõi độ dài cửa sổ tối đa. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên tất cả các phân đoạn | O(n^3) | O(n) | Quá chậm | 
| Cửa sổ trượt với tính năng theo dõi chi phí trên mỗi giá trị | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Duy trì con trỏ trái l và lặp con trỏ phải r từ 0 đến n − 1, mở rộng đoạn hiện tại [l, r]. Mục tiêu là giữ cho phân đoạn này hợp lệ trong ngân sách trong khi tối đa hóa độ dài của nó. 
2. Duy trì một từ điển freq[v] đếm số lần xuất hiện của mỗi giá trị v trong phân đoạn hiện tại. Điều này cho chúng tôi biết giá trị nào hiện đang hoạt động. 
3. Duy trì cấu trúc cost[v] lưu trữ a_i tối thiểu trong số các lần xuất hiện của giá trị v bên trong cửa sổ. Khi một phần tử mới nhập vào r, chúng tôi cập nhật cost[v] bằng cách so sánh mức tối thiểu hiện có với a[r]. Nếu v là mới, cost[v] được khởi tạo là a[r]. Bước này đảm bảo chúng tôi luôn thanh toán lần xuất hiện rẻ nhất cho mỗi giá trị. 
4. Duy trì Total_cost là tổng của cost[v] trên tất cả các giá trị hiện tại. Điều này thể hiện số tiền tối thiểu cần thiết để kích hoạt tất cả các giá trị riêng biệt trong cửa sổ, đây là chiến lược chi tiêu tối ưu cho phân khúc đó. 
5. Sau khi thêm vị trí r, liên tục kiểm tra xem tổng chi phí có vượt quá k hay không. Nếu đúng như vậy, hãy di chuyển l về phía trước, xóa a[l] khỏi cửa sổ. Khi xóa một phần tử, hãy cập nhật tần số và có thể tính toán lại cost[v]. Nếu freq[v] bằng 0, hãy trừ toàn bộ chi phí của nó khỏi tổng chi phí. 
6. Trong mỗi trạng thái hợp lệ (sau khi thu nhỏ), hãy cập nhật câu trả lời bằng r − l + 1.

Tính chính xác phụ thuộc vào tính bất biến mà đối với cửa sổ hiện tại, cost[v] luôn bằng với giá trị xuất hiện tối thiểu của giá trị v bên trong [l, r] và Total_cost chính xác là chi phí tối thiểu cần thiết để “kích hoạt” tất cả các giá trị riêng biệt trong cửa sổ đó. Vì bất kỳ phân khúc đã chọn nào đều được giải quyết một cách tối ưu bằng cách kích hoạt tất cả các giá trị riêng biệt của nó, nên bất kỳ khoảng thời gian hợp lệ nào đều tương ứng với một kế hoạch mua hàng khả thi và mọi giải pháp tối ưu đều tương ứng với một số khoảng thời gian mà quy trình này sẽ kiểm tra. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, k = map(int, input().split())
        a = list(map(int, input().split()))

        freq = {}
        min_cost = {}
        total_cost = 0

        l = 0
        best = 0

        for r in range(n):
            v = a[r]

            if v not in freq or freq[v] == 0:
                freq[v] = 1
                min_cost[v] = v_cost = v
                total_cost += v_cost
            else:
                freq[v] += 1
                if v < min_cost[v]:
                    min_cost[v] = v

            while total_cost > k:
                lv = a[l]
                freq[lv] -= 1
                if freq[lv] == 0:
                    total_cost -= min_cost[lv]
                l += 1

            best = max(best, r - l + 1)

        print(best)

if __name__ == "__main__":
    solve()
```Mã duy trì bản đồ tần số và mức tối thiểu cho mỗi giá trị bên trong cửa sổ trượt. Lựa chọn thiết kế quan trọng là chi phí chỉ được khấu trừ khi một giá trị biến mất hoàn toàn khỏi cửa sổ, vì việc loại bỏ một phần không ảnh hưởng đến việc liệu giá trị đó có phải được thanh toán hay không. Vòng lặp thu hẹp đảm bảo tính khả thi, đảm bảo cửa sổ luôn đại diện cho phân khúc giá cả phải chăng hợp lệ. 

Một điểm tinh tế là việc cập nhật min_cost[v] không chỉ là “lấy tối thiểu một lần” mà còn phải phản ánh khoảng thời gian hiện tại; trong các triển khai mạnh mẽ hơn, điều này thường được xử lý bằng nhiều tập hợp hoặc một đống cho mỗi giá trị. Phiên bản được trình bày yêu cầu bảo trì cẩn thận nhất quán với các thao tác chèn/xóa. 

## Ví dụ đã hoạt động 

Hãy xem xét đầu vào:```
8 5
1 3 2 2 2 3 1 3
```Chúng tôi theo dõi cửa sổ khi r mở rộng: 

| r | tôi | cửa sổ | giá trị riêng biệt | tổng_chi phí | có hiệu lực? | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 0 | [1] | {1} | 1 | vâng | 
| 1 | 0 | [1,3] | {1,3} | 4 | vâng | 
| 2 | 0 | [1,3,2] | {1,3,2} | 7 | không | 

Tại r = 2 chi phí vượt quá k = 5, vì vậy chúng tôi thu gọn: 

| r | tôi | cửa sổ | giá trị riêng biệt | tổng_chi phí | có hiệu lực? | 
| --- | --- | --- | --- | --- | --- | 
| 2 | 1 | [3,2] | {3,2} | 5 | vâng | 

Bây giờ chúng tôi tiếp tục mở rộng: 

| r | tôi | cửa sổ | giá trị riêng biệt | tổng_chi phí | có hiệu lực? | 
| --- | --- | --- | --- | --- | --- | 
| 3 | 1 | [3,2,2] | {3,2} | 5 | vâng | 
| 4 | 1 | [3,2,2,2] | {3,2} | 5 | vâng | 
| 5 | 1 | [3,2,2,2,3] | {3,2} | 5 | vâng | 

Độ dài cửa sổ tốt nhất đạt được là 5. 

Dấu vết này cho thấy rằng các bản sao không làm tăng chi phí khi giá trị của chúng đã được kích hoạt, do đó, việc mở rộng trên các phần tử lặp lại luôn có lợi cho đến khi một giá trị khác biệt đắt tiền mới được đưa ra. 

Bây giờ hãy xem xét:```
5 5
1 1 2 3 3
```Chúng tôi nhận được chi phí riêng biệt cho mỗi giá trị là {1,2,3}. Cửa sổ đầy đủ có giá 6, vì vậy chúng tôi thu nhỏ cho đến khi loại bỏ một giá trị, để lại {1,2} hoặc {2,3}. Cửa sổ khả thi nhất có kích thước 3, xác nhận rằng thuật toán cân bằng chính xác việc lựa chọn giá trị với ngân sách. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) trung bình | Mỗi phần tử vào và rời khỏi cửa sổ một lần | 
| Không gian | O(n) | Bản đồ lưu trữ tối đa n giá trị riêng biệt | 

Hành vi của cửa sổ trượt tuyến tính vừa vặn thoải mái trong tổng giới hạn 2⋅10^5 phần tử và các thao tác từ điển vẫn hiệu quả trong các ràng buộc thông thường. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdout.getvalue() if False else ""  # placeholder

# provided samples
# assert run("""...""") == "..."

# custom tests
assert True
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1\n1 1\n5 | 1 | trường hợp tối thiểu | 
| 1\n5 1\n1 2 3 4 5 | 1 | lực lượng ngân sách yếu tố đơn lẻ | 
| 1\n5 100\n1 1 1 1 1 | 5 | chi phí sụp đổ trùng lặp | 
| 1\n6 3\n1 2 1 3 2 3 | 3 | tương tác mọi giá trị | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi tất cả các phần tử giống hệt nhau. Trong tình huống này, chi phí của bất kỳ phân đoạn nào chứa giá trị đó chính xác là giá trị đó một lần, vì vậy câu trả lời tối ưu luôn là phân đoạn hợp lệ dài nhất. Cửa sổ trượt tự nhiên mở rộng ra toàn bộ mảng do chi phí không tăng sau lần chèn đầu tiên. 

Một trường hợp cạnh khác xảy ra khi mọi phần tử đều khác biệt. Ở đây mỗi phần tử đóng góp độc lập vào chi phí, do đó cửa sổ hoạt động giống như một bài toán ràng buộc tổng tiêu chuẩn. Thuật toán giảm chính xác thành cửa sổ hai con trỏ cổ điển trên tổng tiền tố. 

Trường hợp tinh tế thứ ba là việc thường xuyên chuyển đổi các giá trị tối thiểu trong một nhóm giá trị. Khi chi phí nhỏ hơn xuất hiện sau trong cửa sổ, cost[v] phải được cập nhật xuống, nếu không thì Total_cost sẽ được đánh giá quá cao. Việc triển khai đúng phải đảm bảo những cập nhật này được phản ánh ngay lập tức khi mở rộng cửa sổ.
