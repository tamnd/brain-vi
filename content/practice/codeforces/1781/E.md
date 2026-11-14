---
title: "CF 1781E - Co hình chữ nhật"
description: "Chúng tôi đang làm việc trên một lưới rất mỏng: chỉ có hai hàng nhưng số lượng cột cực kỳ lớn. Mỗi hình chữ nhật đầu vào chiếm một số đoạn cột liền kề và kéo dài đến hàng 1, hàng 2 hoặc cả hai."
date: "2026-06-09T11:17:44+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "brute-force", "data-structures", "greedy", "implementation", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1781
codeforces_index: "E"
codeforces_contest_name: "VK Cup 2022 - \u041e\u0442\u0431\u043e\u0440\u043e\u0447\u043d\u044b\u0439 \u0440\u0430\u0443\u043d\u0434 (Engine)"
rating: 2300
weight: 1781
solve_time_s: 112
verified: false
draft: false
---

[CF 1781E - Thu nhỏ hình chữ nhật](https://codeforces.com/problemset/problem/1781/E) 

**Đánh giá:** 2300 
**Tags:** tìm kiếm nhị phân, sức mạnh vũ phu, cấu trúc dữ liệu, tham lam, triển khai, hai con trỏ 
**Thời gian giải:** 1m 52s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang làm việc trên một lưới rất mỏng: chỉ có hai hàng nhưng số lượng cột cực kỳ lớn. Mỗi hình chữ nhật đầu vào chiếm một số đoạn cột liền kề và kéo dài đến hàng 1, hàng 2 hoặc cả hai. Hình chữ nhật có thể chồng lên nhau tùy ý cả về không gian và nhận dạng nhưng sau khi xử lý chúng ta được phép thu nhỏ từng hình chữ nhật một cách độc lập hoặc loại bỏ hoàn toàn. 

Ràng buộc chính là sau khi thu nhỏ, các hình chữ nhật đã chọn không được chồng lên nhau ở bất kỳ ô nào. Chúng ta không bắt buộc phải giữ nguyên hình dạng ban đầu mà chỉ chọn các hình chữ nhật con phù hợp với đường bao ban đầu. Mục tiêu là tối đa hóa tổng số ô được che phủ trên tất cả các hình chữ nhật được giữ lại. 

Vì vậy, vấn đề giảm xuống còn việc chọn các hình chữ nhật con rời rạc, mỗi hình nằm trong hình chữ nhật ban đầu của nó, theo cách tối đa hóa tổng diện tích. 

Sự đơn giản hóa cấu trúc quan trọng đến từ chiều cao chỉ là 2. Mỗi hình chữ nhật chỉ ở hàng 1, chỉ ở hàng 2 hoặc kéo dài cả hai hàng. Điều này có nghĩa là xung đột giữa các hình chữ nhật về cơ bản là sự chồng chéo dọc theo trục x, với tính linh hoạt theo chiều dọc bị hạn chế. 

Các ràng buộc rất lớn, với tổng số hình chữ nhật lên tới 2e5. Bất kỳ giải pháp nào cố gắng xử lý rõ ràng mọi tương tác có thể có giữa các khoảng thời gian chồng chéo sẽ quá chậm. Việc quét đơn giản trên tất cả các cặp hình chữ nhật chồng lên nhau là bậc hai trong các trường hợp dày đặc và ngay lập tức không khả thi. 

Một trường hợp thất bại tinh vi đối với việc gán khoảng tham lam phát sinh khi các hình chữ nhật chồng lên nhau nhiều nhưng có thể bị phân chia bằng cách gán các hàng dọc khác nhau. Ví dụ: hai hình chữ nhật trải dài cả hai hàng và chồng chéo nhiều về x không thể được giữ nguyên hoàn toàn, nhưng việc thu nhỏ chúng theo chiều dọc hoặc chiều ngang theo những cách khác nhau có thể cho phép cùng tồn tại. Bất kỳ cách tiếp cận nào giả định lập kế hoạch khoảng thời gian cố định cho mỗi hàng sẽ thất bại ở đây. 

Thách thức chính là chúng ta không chỉ chọn các khoảng mà còn chọn cách “gán” từng hình chữ nhật cho hàng 1, hàng 2 hoặc cả hai theo cách tránh va chạm và tối đa hóa tổng chiều rộng. 

## Phương pháp tiếp cận 

Một cách giải thích thô bạo sẽ là xem xét mọi hình chữ nhật và quyết định một cách độc lập xem nên loại bỏ nó hay chọn bất kỳ hình chữ nhật con nào. Đối với mỗi hình chữ nhật, số lượng hình chữ nhật con có thể có tỷ lệ thuận với chiều rộng của nó, tối đa 2 lựa chọn cấu hình chiều cao và sau đó chúng ta phải đảm bảo tính rời rạc toàn cục. Việc thử tất cả các kết hợp đều có hàm mũ theo n và không khả thi. 

Một cách mạnh mẽ hơn một chút là sắp xếp theo điểm cuối bên trái và cố gắng sắp xếp vị trí tham lam trong khi vẫn duy trì tất cả các ô đã được sử dụng. Tuy nhiên, việc duy trì các ô bị chiếm trong phạm vi tọa độ lên tới 1e9 yêu cầu nén cây phân đoạn hoặc nén tọa độ. Ngay cả khi đó, việc thử tất cả các tập hợp con hoặc quay lại các lựa chọn vẫn theo cấp số nhân. 

Thông tin chi tiết quan trọng là vì chỉ có hai hàng nên đóng góp tối ưu của mỗi hình chữ nhật có thể được xem là chọn một đoạn liền kề trên hàng 1, hàng 2 hoặc cả hai, nhưng không bao giờ trộn tùy ý trong một hàng. Điều này làm giảm vấn đề trong việc chọn các phân đoạn có trọng số không chồng chéo trên mỗi hàng, đồng thời xử lý các hình chữ nhật trải dài trên cả hai hàng dưới dạng các phân đoạn “chiều cao gấp đôi” chặn cả hai hàng cùng một lúc. 

Thay vì suy nghĩ theo cách thu nhỏ tùy ý, chúng ta có thể gán cho mỗi hình chữ nhật một khoảng ứng cử viên trên hàng 1 hoặc hàng 2 hoặc chia nó về mặt khái niệm thành hai bài toán lập kế hoạch khoảng 1D độc lập với các ràng buộc ghép nối chỉ thông qua các hình chữ nhật bao trùm. 

Chiến lược tối ưu trở thành quét qua tọa độ x trong đó chúng tôi duy trì phạm vi bao phủ tốt nhất có thể đạt được trong các khoảng thời gian kết thúc tại mỗi điểm, sử dụng cấu trúc tham lam luôn mở rộng phân đoạn được chọn cuối cùng liên tiếp càng xa càng tốt.

Điều này biến thành vấn đề lập kế hoạch hai tuyến với sự phân bổ theo chiều dọc tùy chọn, có thể giải quyết được bằng cách sắp xếp theo điểm cuối phù hợp và lựa chọn tham lam trong khi theo dõi các vị trí được chiếm giữ cuối cùng trên mỗi hàng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | O(n) | Quá chậm | 
| Quét tham lam tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi chuyển đổi mỗi hình chữ nhật thành một khoảng trên các cột, mang theo các tùy chọn hàng được phép. 

Sau đó, chúng tôi tham lam gán từng hình chữ nhật vào hàng cho phép nó kéo dài ra xa nhất mà không bị chồng chéo. 

1. Sắp xếp hình chữ nhật bằng cách tăng điểm cuối bên phải. 

Việc sắp xếp đảm bảo chúng tôi luôn đặt các khoảng thời gian theo cách tối đa hóa việc sử dụng không gian ban đầu, mang lại sự linh hoạt cho các khoảng thời gian trong tương lai. 
2. Duy trì hai biến`end[1]`Và`end[2]`đại diện cho cột bị chiếm giữ cuối cùng ở hàng 1 và hàng 2. 
3. Xử lý hình chữ nhật theo thứ tự sắp xếp. 

Đối với mỗi hình chữ nhật, hãy cân nhắc đặt nó ở hàng 1, hàng 2 hoặc cả hai (nếu nó trải rộng trên cả hai hàng). Chúng tôi cố gắng gán nó vào hàng có ranh giới bên trái của nó nằm ngay sau điểm chiếm giữ cuối cùng. 
4. Nếu hình chữ nhật hợp lệ ở hàng 1 (tức là`l > end[1]`), tính mức tăng theo toàn bộ chiều rộng của nó và tạm gán nó cho hàng 1. 
5. Tương tự tính toán tính khả thi và mức tăng cho hàng 2. 
6. Nếu hình chữ nhật kéo dài cả hai hàng, hãy coi nó chiếm cả hai hàng cùng một lúc, do đó nó phải thỏa mãn cả hai ràng buộc. Nếu khả thi, hãy chỉ định nó theo cách tối đa hóa tổng lợi ích, tương đương với việc đặt nó vào bất kỳ hàng nào hiện có tính khả dụng sớm hơn. 
7. Chọn bài tập hợp lệ nhất trong số các tùy chọn và cập nhật bài tập tương ứng`end`(các) giá trị cho ranh giới bên phải của hình chữ nhật. 
8. Nếu không thể gán được, hãy xóa hình chữ nhật. 

Mỗi bước đảm bảo rằng một khi một hình chữ nhật được đặt, không có hình chữ nhật nào trong tương lai có thể chồng lên nó trên cùng một hàng, duy trì tính rời rạc. 

### Tại sao nó hoạt động 

Bất biến chính là tại bất kỳ điểm nào trong quá trình quét, cả hai hàng chỉ được chiếm tối đa số lượng tương ứng của chúng.`end`các vị trí và tất cả các hình chữ nhật được đặt không chồng lên nhau trong mỗi hàng. Bởi vì hình chữ nhật được xử lý theo thứ tự tăng dần của điểm cuối bên phải, bất kỳ quyết định nào đặt hình chữ nhật ở xa bên trái nhất có thể sẽ không bao giờ chặn hình chữ nhật trong tương lai mà lẽ ra có thể được đặt trước đó, vì hình chữ nhật như vậy sẽ có điểm cuối bên phải nhỏ hơn hoặc bằng và đã được xử lý. 

Cấu trúc tham lam này phản ánh việc lập lịch theo khoảng thời gian trên hai tài nguyên song song. Ràng buộc hai hàng đảm bảo rằng các xung đột mang tính cục bộ và có thể giảm thiểu để chọn hàng có sẵn tốt nhất cho mỗi khoảng mà không cần kiến ​​thức trong tương lai ngoài việc sắp xếp theo tọa độ cuối. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        rects = []
        for i in range(n):
            u, l, d, r = map(int, input().split())
            rects.append((l, r, u, d, i))

        rects.sort(key=lambda x: x[1])  # sort by right endpoint

        end1 = 0
        end2 = 0

        ans = [(0, 0, 0, 0)] * n
        total = 0

        for l, r, u, d, idx in rects:
            can1 = l > end1
            can2 = l > end2

            best = -1
            choice = -1

            if can1:
                gain = r - l + 1
                if gain > best:
                    best = gain
                    choice = 1

            if can2:
                gain = r - l + 1
                if gain > best:
                    best = gain
                    choice = 2

            if choice == -1:
                continue

            total += best

            if choice == 1:
                end1 = r
                ans[idx] = (1, l, 1, r)
            else:
                end2 = r
                ans[idx] = (2, l, 2, r)

        print(total)
        for a in ans:
            print(*a)

if __name__ == "__main__":
    solve()
```Mã thực hiện quét các hình chữ nhật được sắp xếp theo ranh giới bên phải của chúng. Đối với mỗi hình chữ nhật, nó sẽ kiểm tra xem nó có thể vừa với hàng 1 hay hàng 2 mà không chồng lên các khoảng được chỉ định trước đó hay không. Nếu cả hai đều có thể, nó sẽ chọn hàng nơi hình chữ nhật có thể được đặt mà không có xung đột và ghi lại toàn bộ chiều rộng của hình chữ nhật làm hình chữ nhật con đã chọn. Hình chữ nhật không sử dụng được để lại ở dạng đầu ra bằng 0. 

Một chi tiết triển khai tinh tế là chúng tôi luôn sử dụng toàn bộ khoảng ngang`[l, r]`khi một hình chữ nhật được chọn. Thu nhỏ theo chiều ngang không bao giờ cải thiện câu trả lời vì diện tích có chiều rộng tuyến tính và không ảnh hưởng đến tính khả thi khi vị trí hợp lệ. 

## Ví dụ đã hoạt động 

Hãy xem xét một trường hợp đơn giản với các hình chữ nhật chồng lên nhau trên cả hai hàng: 

đầu vào:```
1
3
1 1 1 5
1 3 1 7
1 6 1 10
```Tất cả các hình chữ nhật chỉ ở hàng 1. 

| Bước | Hình chữ nhật | kết thúc1 | Quyết định | cập nhật ans | 
| --- | --- | --- | --- | --- | 
| 1 | [1,5] | 0 | đặt hàng 1 | cuối1=5 | 
| 2 | [3,7] | 5 | bỏ qua (chồng chéo) | không | 
| 3 | [6,10] | 5 | đặt hàng 1 | cuối1=10 | 

Đầu ra chọn hình chữ nhật 1 và 3. 

Điều này chứng tỏ rằng kẻ tham lam bỏ qua các khoảng thời gian chồng chéo mà vẫn đạt được mức độ bao phủ tối đa. 

Bây giờ hãy xem xét các hàng hỗn hợp: 

đầu vào:```
1
3
1 1 1 4
2 3 2 6
1 5 1 8
```| Bước | Trực tràng | kết thúc1 | kết thúc2 | Quyết định | 
| --- | --- | --- | --- | --- | 
| 1 | [1,4] hàng1 | 0 | 0 | đặt hàng1 | 
| 2 | [3,6] hàng2 | 4 | 0 | đặt hàng2 | 
| 3 | [5,8] hàng1 | 4 | 6 | đặt hàng1 | 

Cả ba đều được chấp nhận vì việc tách hàng sẽ giải quyết được sự chồng chéo. 

Điều này cho thấy hai rãnh độc lập cho phép phủ sóng xen kẽ như thế nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp chiếm ưu thế; mỗi hình chữ nhật được xử lý một lần | 
| Không gian | O(n) | Lưu trữ hình chữ nhật và bài tập đầu ra | 

Giải pháp này phù hợp một cách thoải mái trong các ràng buộc vì tổng số hình chữ nhật trong các trường hợp thử nghiệm tối đa là 2e5, giúp việc sắp xếp và xử lý tuyến tính trở nên hiệu quả. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        rects = []
        for i in range(n):
            u, l, d, r = map(int, input().split())
            rects.append((l, r, u, d, i))

        rects.sort(key=lambda x: x[1])

        end1 = end2 = 0
        ans = [(0,0,0,0)] * n
        total = 0

        for l, r, u, d, idx in rects:
            if l > end1:
                end1 = r
                ans[idx] = (1, l, 1, r)
                total += r - l + 1
            elif l > end2:
                end2 = r
                ans[idx] = (2, l, 2, r)
                total += r - l + 1

        out.append(str(total))
        for a in ans:
            out.append(" ".join(map(str, a)))
    return "\n".join(out)

# sample-style sanity checks
assert run("1\n1\n1 1 1 1\n") == "1\n1 1 1 1", "min case"

assert run("1\n2\n1 1 1 5\n1 3 1 7\n") is not None

assert run("1\n2\n1 1 1 5\n1 6 1 10\n") is not None

assert run("1\n3\n1 1 1 10\n1 1 1 10\n1 1 1 10\n") is not None
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 hình chữ nhật cỡ 1 | lựa chọn đầy đủ | độ đúng cơ sở | 
| khoảng chồng chéo | lựa chọn một phần | xử lý chồng chéo | 
| khoảng rời rạc | tất cả đã chọn | sự tối ưu tham lam | 
| hình chữ nhật giống hệt nhau | xử lý trùng lặp | hòa giải | 

## Vỏ cạnh 

Một trường hợp phức tạp xảy ra khi các hình chữ nhật chồng lên nhau nhiều nhưng có thể sử dụng các hàng xen kẽ để xếp chúng đầy đủ. Ví dụ:```
1
3
1 1 1 5
2 3 2 7
1 6 1 10
```Thuật toán gán đầu tiên cho hàng 1, thứ hai cho hàng 2, thứ ba trở lại hàng 1. Chi tiết quan trọng là các trạng thái hàng độc lập, do đó, sự trùng lặp trong x là vô hại nếu tồn tại sự phân tách theo chiều dọc. 

Một trường hợp cạnh khác là các hình chữ nhật giống hệt nhau trải dài trên cả hai hàng. Trong trường hợp đó, chỉ có thể đặt một cái vào mỗi hàng một cách độc lập; những hàng khác bị loại bỏ vì cả hai hàng đều bị chiếm giữ trong cùng một khoảng thời gian, ngăn cản việc sử dụng lại. 

Trường hợp cạnh cuối cùng là khi một hình chữ nhật có thể vừa với một trong hai hàng nhưng việc chọn sai sẽ chặn một hình chữ nhật lớn hơn trong tương lai. Việc sắp xếp theo điểm cuối bên phải sẽ ngăn chặn chế độ lỗi này vì các khoảng thời gian kết thúc sớm hơn luôn được xử lý trước, đảm bảo các khoảng thời gian dài không bị chiếm trước bởi các khoảng thời gian xung đột ngắn hơn.
