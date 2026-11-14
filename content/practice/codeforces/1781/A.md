---
title: "CF 1781A - Phép chiếu song song"
description: "Chúng ta đang làm việc bên trong một căn phòng hình chữ nhật có dạng hình hộp. Một điểm trên sàn đánh dấu nơi đặt máy tính xách tay và một điểm ngay phía trên trần nhà đánh dấu nơi treo máy chiếu."
date: "2026-06-09T11:15:48+07:00"
tags: ["codeforces", "competitive-programming", "geometry", "math"]
categories: ["algorithms"]
codeforces_contest: 1781
codeforces_index: "A"
codeforces_contest_name: "VK Cup 2022 - \u041e\u0442\u0431\u043e\u0440\u043e\u0447\u043d\u044b\u0439 \u0440\u0430\u0443\u043d\u0434 (Engine)"
rating: 800
weight: 1781
solve_time_s: 120
verified: false
draft: false
---

[CF 1781A - Phép chiếu song song](https://codeforces.com/problemset/problem/1781/A) 

**Đánh giá:** 800 
**Thẻ:** hình học, toán học 
**Thời gian giải:** 2 phút 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta đang làm việc bên trong một căn phòng hình chữ nhật có dạng hình hộp. Một điểm trên sàn đánh dấu nơi đặt máy tính xách tay và một điểm ngay phía trên trần nhà đánh dấu nơi treo máy chiếu. Cáp phải kết nối hai điểm này, nhưng nó bị hạn chế chỉ di chuyển dọc theo bề mặt của căn phòng, nghĩa là nó có thể di chuyển trên sàn, tường và trần nhà và phải luôn đi theo hướng song song với các cạnh của hộp. 

Nhiệm vụ là tính toán độ dài cáp ngắn nhất có thể giữa máy tính xách tay và máy chiếu theo những hạn chế này. Cáp không thể cắt xuyên qua phần bên trong của hộp, vì vậy bất kỳ đường dẫn nào cũng phải bao gồm các đoạn thẳng hàng với trục nằm trên các mặt của hình hộp chữ nhật. 

Ràng buộc chính là hình học chứ không phải tổ hợp. Chúng tôi đang tìm kiếm một cách hiệu quả đường đi ngắn nhất kiểu Manhattan trên bề mặt của hộp 3D giữa hai điểm nằm trên các mặt đối diện. 

Kích thước đầu vào nhỏ cho mỗi trường hợp thử nghiệm, tối đa 10^4 trường hợp, vì vậy mỗi trường hợp phải được giải trong thời gian không đổi. Điều này loại trừ bất kỳ tìm kiếm hoặc mô phỏng biểu đồ nào trên bề mặt. Giải pháp phải xuất phát từ việc thừa nhận rằng chỉ có một số lượng nhỏ cố định các bề mặt mở ra có thể ứng cử viên mới là quan trọng. 

Trường hợp cạnh tinh tế xuất hiện khi các hình chiếu ngang của hai điểm thẳng hàng trong một tọa độ. Trong những trường hợp như vậy, nhiều đường dẫn ứng cử viên sẽ thu gọn lại thành cùng một hình dạng và các công thức đơn giản giả định các tọa độ riêng biệt có thể bị tính quá mức hoặc bỏ lỡ tuyến đường tối ưu. 

## Phương pháp tiếp cận 

Chế độ xem brute-force coi bề mặt hình khối như một biểu đồ: mọi điểm trên các cạnh và mặt là một nút và chuyển động dọc theo các mặt được cho phép với chi phí Manhattan. Người ta có thể rời rạc hóa bề mặt thành lưới và chạy thuật toán đường đi ngắn nhất. Về nguyên tắc, điều này đúng, nhưng số lượng trạng thái tăng lên cùng với độ phân giải của sự rời rạc hóa, và ngay cả sự rời rạc hóa thô cũng không cần thiết vì các điểm cuối là tọa độ liên tục. Điều này làm cho vũ lực không thể sử dụng được. 

Quan sát quan trọng là đường đi tối ưu giữa hai điểm trên bề mặt hộp luôn tương ứng với việc mở hai mặt liền kề thành một mặt phẳng. Sau khi được mở ra, đường đi sẽ trở thành một đường thẳng khoảng cách Manhattan giữa hai điểm phẳng. Chỉ có một số lượng không đổi các lần mở có ý nghĩa và mỗi lần tương ứng với việc chọn một cặp mặt liền kề mà cáp đi qua. 

Đối với điểm sàn (a, b, 0) và điểm trần (f, g, h), đường đi tối ưu luôn bao gồm việc di chuyển từ sàn lên một bức tường thẳng đứng rồi băng qua trần nhà. Con đường ngắn nhất chỉ phụ thuộc vào việc chúng ta đi qua các bức tường bên trái, bên phải, phía trước hay phía sau, điều này dẫn đến một vài biểu thức khác biệt tuyệt đối. 

Điều này làm giảm vấn đề đánh giá một tập hợp nhỏ các khoảng cách Manhattan ứng cử viên và lấy mức tối thiểu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tìm kiếm đồ thị bề mặt | O(∞ rời rạc hóa) | O(lớn) | Quá chậm | 
| Mở ra + phân tích trường hợp | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi bắt đầu bằng cách quan sát rằng mọi đường dẫn hợp lệ đều phải di chuyển từ sàn đến tường, sau đó có thể xuyên qua bức tường đó và cuối cùng lên trần nhà. Vì chuyển động bị hạn chế ở các cạnh thẳng hàng theo trục trên các mặt, nên mọi đường dẫn tương ứng với việc chọn hướng mà chúng ta “thoát” khỏi hình chữ nhật sàn.

1. Tính khoảng cách ngang trong mặt phẳng sàn, nghĩa là |a - f| và |b - g|. Chúng thể hiện khoảng cách hình chiếu của hai điểm dọc theo hai trục sàn. 
2. Hãy xem xét bốn cách có thể để di chuyển giữa sàn và trần bằng cách sử dụng các bức tường thẳng hàng với các cạnh hình chữ nhật. Mỗi lựa chọn tương ứng với việc định tuyến đường đi quanh một cạnh của hộp. 
3. Đối với mỗi tuyến đường dự kiến, hãy chuyển đổi đường dẫn 3D thành biểu thức khoảng cách Manhattan 2D. Mỗi tuyến đường đóng góp tổng cộng một chênh lệch theo chiều ngang, một chênh lệch theo chiều dọc và chiều cao h. 
4. Đánh giá tất cả các biểu thức ứng viên và lấy giá trị nhỏ nhất. 

Lý do chúng ta chỉ cần một số lượng biểu thức không đổi là vì bất kỳ đường dẫn bề mặt ngắn nhất nào cũng phải tương ứng với một mặt mở của hai mặt liền kề và chỉ có bốn mặt mở như vậy nối từ sàn đến trần thông qua một ranh giới tường duy nhất. 

### Tại sao nó hoạt động 

Bất kỳ đường đi bề mặt hợp lệ nào từ sàn đến trần đều phải đi qua đúng một đường chuyển tiếp thẳng đứng từ z = 0 đến z = h qua một bức tường. Khi điểm chuyển tiếp đó được cố định trên một cạnh của hộp, phần còn lại của đường dẫn trên mỗi mặt sẽ là đoạn thẳng Manhattan. Việc giảm thiểu tất cả các cạnh tường có thể giúp giảm việc so sánh một tập hợp cấu hình không đổi và không có đường vòng bên trong nào có thể cải thiện đường đi Manhattan trên một mặt phẳng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        w, d, h = map(int, input().split())
        a, b, f, g = map(int, input().split())

        dx = abs(a - f)
        dy = abs(b - g)

        # four candidate surface routes
        ans = min(
            dx + b + g + h,
            dx + (d - b) + (d - g) + h,
            dy + a + f + h,
            dy + (w - a) + (w - f) + h
        )

        print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai tính toán trước sự khác biệt theo chiều ngang và đánh giá bốn cấu hình hình học. Mỗi biểu thức tương ứng với việc chọn một trong bốn cạnh thẳng đứng của căn phòng làm bề mặt chuyển tiếp giữa sàn và trần. Các số hạng được thêm vào như b + g hoặc (d - b) + (d - g) thể hiện việc đi bộ từ các điểm đến cạnh tường đã chọn trước khi đi theo phương thẳng đứng qua độ cao h. 

Một lỗi phổ biến là quên rằng quá trình chuyển đổi có thể xảy ra trên bất kỳ bức tường nào trong số bốn bức tường, không chỉ bức tường gần nhất trong hình chiếu. Một cách khác là giả định khoảng cách trực tiếp của Manhattan cộng với chiều cao, điều này không thành công khi cả hai điểm đều gần một bức tường khác hơn so với phép chiếu ngây thơ gợi ý. 

## Ví dụ đã hoạt động 

Chúng tôi theo dõi hai trường hợp từ bộ mẫu. 

### Ví dụ 1 

Đầu vào: w=55, d=20, h=29, a=23, b=10, f=18, g=3 

| Biểu hiện | Giá trị | 
| --- | --- | 
| dx + b + g + h | 5 + 10 + 3 + 29 = 47 | 
| dx + (d-b) + (d-g) + h | 5 + 10 + 17 + 29 = 61 | 
| dy + a + f + h | 7 + 23 + 18 + 29 = 77 | 
| dy + (w-a) + (w-f) + h | 7 + 32 + 37 + 29 = 105 | 

Tối thiểu là 47. 

Điều này xác nhận rằng việc định tuyến thông qua hình chiếu ngang gần nhất lên một bức tường bên sẽ chiếm ưu thế hơn tất cả các lựa chọn thay thế. 

### Ví dụ 2 

Đầu vào: w=20, d=10, h=5, a=1, b=5, f=2, g=5 

| Biểu hiện | Giá trị | 
| --- | --- | 
| dx + b + g + h | 1 + 5 + 5 + 5 = 16 | 
| dx + (d-b) + (d-g) + h | 1 + 5 + 5 + 5 = 16 | 
| dy + a + f + h | 0 + 1 + 2 + 5 = 8 | 
| dy + (w-a) + (w-f) + h | 0 + 19 + 18 + 5 = 42 | 

Tối thiểu là 8. 

Điều này cho thấy trường hợp suy biến trong đó b bằng g, làm cho việc căn chỉnh theo chiều dọc không còn phù hợp và cho phép chuyển đổi bức tường trực tiếp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Mỗi trường hợp thử nghiệm đánh giá một số lượng biểu thức số học không đổi | 
| Không gian | O(1) | Chỉ một vài biến được lưu trữ cho mỗi trường hợp thử nghiệm | 

Các ràng buộc cho phép tối đa 10^4 trường hợp thử nghiệm, do đó cần có giải pháp O(1) cho mỗi trường hợp. Việc đánh giá hình học theo thời gian không đổi phù hợp thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    def solve():
        t = int(input())
        out = []
        for _ in range(t):
            w, d, h = map(int, input().split())
            a, b, f, g = map(int, input().split())

            dx = abs(a - f)
            dy = abs(b - g)

            out.append(str(min(
                dx + b + g + h,
                dx + (d - b) + (d - g) + h,
                dy + a + f + h,
                dy + (w - a) + (w - f) + h
            )))
        return "\n".join(out)

    return solve()

# provided samples
assert run("""5
55 20 29
23 10 18 3
20 10 5
1 5 2 5
15 15 4
7 13 10 10
2 1000 2
1 1 1 999
10 4 10
7 1 2 1
""") == """47
8
14
1002
17"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| cùng căn chỉnh x,y | giá trị nhỏ | liên kết dọc thoái hóa | 
| vị trí đối xứng | ứng viên bình đẳng | tính đối xứng của các bức tường | 
| định tuyến góc cực | giá trị lớn | tính đúng đắn của đường biên | 

## Vỏ cạnh 

Khi cả hai điểm chia sẻ cùng một hình chiếu trên một trục, một số tuyến ứng cử viên sẽ sụp đổ và nhiều biểu thức trở nên bằng nhau. Thuật toán xử lý điều này một cách tự nhiên vì sự khác biệt tuyệt đối trở thành 0 và mức tối thiểu vẫn chọn đường dẫn tường hợp lệ. 

Khi một điểm đã ở gần một ranh giới, các biểu thức liên quan đến bức tường đó sẽ giảm đáng kể và mức tối thiểu sẽ chọn đúng phía đó. Cấu trúc công thức đảm bảo rằng không có lối tắt không hợp lệ nào xuyên qua bên trong được xem xét, vì mọi ứng cử viên đều thực thi một cách rõ ràng việc truyền tải bề mặt qua một bức tường cộng với chuyển tiếp theo chiều dọc.
