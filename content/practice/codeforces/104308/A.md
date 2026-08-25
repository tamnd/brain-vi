---
title: "CF 104308A - Mưa Mưa Đi Đi, Ngày Lại Đến!"
description: "Đầu vào mô tả một số “cảnh quan” độc lập được tạo thành từ các chồng gạch thẳng đứng có chiều rộng đơn vị. Mỗi cảnh quan là một mảng trong đó giá trị tại vị trí i biểu thị độ cao của bức tường tại thời điểm đó. Khi mưa rơi, nước có thể tích tụ trong khoảng trống giữa các bức tường cao hơn."
date: "2026-07-01T20:01:16+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104308
codeforces_index: "A"
codeforces_contest_name: "Mirror of Independence Day Programming Contest 2023 by MIST Computer Club"
rating: 0
weight: 104308
solve_time_s: 63
verified: true
draft: false
---

[CF 104308A - Mưa đi đi, ngày khác lại đến!](https://codeforces.com/problemset/problem/104308/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 3s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Đầu vào mô tả một số “cảnh quan” độc lập được tạo thành từ các chồng gạch thẳng đứng có chiều rộng đơn vị. Mỗi cảnh quan là một mảng trong đó giá trị tại vị trí`i`thể hiện chiều cao của bức tường tại thời điểm đó. 

Khi mưa rơi, nước có thể tích tụ trong khoảng trống giữa các bức tường cao hơn. Nước chỉ bị giữ lại khi có những bức tường cao hơn hoặc bằng nhau ở cả bên trái và bên phải của một vị trí, nếu không nước sẽ chảy ra ngoài. 

Đối với mỗi trường hợp thử nghiệm, nhiệm vụ là tính toán tổng lượng nước còn lại sau mưa trên toàn bộ cấu trúc. 

Các ràng buộc ngụ ý lên tới 10.000 trường hợp thử nghiệm, nhưng tổng số vị trí trên tất cả các trường hợp nhiều nhất là 20.000. Giới hạn duy nhất đó chính là chìa khóa: bất kỳ giải pháp nào gần với tuyến tính cho mỗi trường hợp thử nghiệm sẽ quá chậm, nhưng việc quét tuyến tính trên mỗi phần tử nói chung là có thể chấp nhận được. Bất cứ điều gì bậc hai cho mỗi trường hợp thử nghiệm sẽ vượt xa giới hạn. 

Một số trường hợp đặc biệt quan trọng đối với tính chính xác. 

Một mảng tăng nghiêm ngặt như`[1, 2, 3, 4]`không có bẫy nước, vì mọi vị trí đều không có ranh giới bên trái nào cao hơn chính nó. Câu trả lời phải là 0. 

Một mảng giảm nghiêm ngặt như`[4, 3, 2, 1]`cũng không bẫy được nước vì lý do tương tự, nhưng một thuật toán ngây thơ chỉ nhìn vào cực đại một phía mà không ghép cặp thích hợp có thể tính không chính xác các đóng góp âm hoặc một phần nếu thực hiện bất cẩn. 

Mảng phẳng như`[2, 2, 2, 2]`cũng bẫy không có nước. Một số cách triển khai không chính xác vô tình coi các ranh giới bằng nhau là không gian bẫy nếu chúng sử dụng các bất đẳng thức nghiêm ngặt thay vì cực đại bao gồm. 

Cuối cùng, các trường hợp giống như một phần tử hoặc cấu trúc trống, chẳng hạn như`[5]`phải trả về 0 vì không có vùng chứa nào có thể hình thành. 

## Phương pháp tiếp cận 

Cách trực tiếp để tính toán lượng nước bị bẫy là kiểm tra từng vị trí một cách độc lập và xác định lượng nước nằm phía trên nó. Đối với một chỉ số nhất định`i`, mực nước bị giới hạn bởi bức tường cao nhất ở bên trái và bức tường cao nhất ở bên phải. Nếu chúng ta biểu thị chúng là`L[i]`Và`R[i]`, thì nước ở trên vị trí`i`là`max(0, min(L[i], R[i]) - h[i])`. 

Việc thực hiện mạnh mẽ sẽ tính toán`L[i]`Và`R[i]`bằng cách quét trái và phải cho mọi chỉ mục. Điều này đúng vì nó tái cấu trúc rõ ràng các ràng buộc trên từng vị trí. Tuy nhiên, mỗi chỉ mục yêu cầu công việc O(n) để tính toán lại ranh giới của nó, dẫn đến O(n²) cho mỗi trường hợp thử nghiệm. Với tổng số`n`lên tới 2×10⁴, hành vi trong trường hợp xấu nhất này trở nên quá chậm trong giới hạn 1 giây chặt chẽ nếu các trường hợp thử nghiệm được phân phối đối nghịch. 

Quan sát quan trọng là cực đại bên trái và bên phải không cần phải tính toán lại nhiều lần. Chúng có thể được tính toán trước một lần. Khi chúng ta có mảng cực đại tiền tố và mảng cực đại hậu tố, mỗi vị trí có thể được đánh giá theo O(1), giảm vấn đề thành quét tuyến tính. Thậm chí, việc tối ưu hóa hơn nữa sẽ loại bỏ hoàn toàn các mảng phụ trợ bằng cách sử dụng kỹ thuật hai con trỏ để duy trì cực đại chạy từ cả hai đầu, đảm bảo mỗi phần tử được xử lý một lần. 

Phương pháp hai con trỏ có hiệu quả vì hệ số giới hạn đối với nước bị mắc kẹt ở bất kỳ vị trí nào luôn là giá trị nhỏ hơn trong số các ranh giới tốt nhất nhìn thấy từ hai phía. Bằng cách luôn thúc đẩy bên có ranh giới nhỏ hơn, chúng tôi đảm bảo rằng quyết định ở bước đó là quyết định cuối cùng và không thể cải thiện sau này. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Quá chậm | 
| Mảng tiền tố/hậu tố | O(n) | O(n) | Đã chấp nhận | 
| Hai con trỏ | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi sử dụng chiến lược hai con trỏ với cực đại đang chạy. 

1. Khởi tạo hai con trỏ`l = 0`Và`r = n - 1`và hai biến`left_max = 0`Và`right_max = 0`. Chúng đại diện cho ranh giới tốt nhất được nhìn thấy từ mỗi phía. 
2. Trong khi`l < r`, so sánh`h[l]`Và`h[r]`. Bên có chiều cao nhỏ hơn sẽ xác định phép tính tiếp theo. 
3. Nếu`h[l] <= h[r]`, chúng tôi xử lý vị trí`l`. Lý do là ranh giới bên phải đã được đảm bảo ít nhất là`h[l]`, vậy hệ số giới hạn là`left_max`. Nếu như`h[l] >= left_max`, chúng tôi cập nhật`left_max`. Nếu không, nước sẽ tích tụ ở`left_max - h[l]`. Sau đó tăng dần`l`. 
4. Nếu không, chúng tôi xử lý vị trí`r`một cách đối xứng. Nếu như`h[r] >= right_max`, cập nhật`right_max`. Nếu không thì thêm`right_max - h[r]`. Sau đó giảm dần`r`. 
5. Tích lũy tất cả các khoản đóng góp trong quá trình quét. 

Ở mỗi bước, chúng tôi luôn chốt mức đóng góp nước cho một ranh giới mà không cần thông tin trong tương lai, vì phía đối diện được đảm bảo đủ cao để tạo thành giới hạn cho chỉ số đó. 

### Tại sao nó hoạt động 

Tính bất biến là tại bất kỳ thời điểm nào, tất cả các vị trí đều nằm ngoài hiện tại`[l, r]`cửa sổ đã được xử lý đầy đủ với các ràng buộc ranh giới chính xác. Quyết định xử lý phía nhỏ hơn đảm bảo rằng đối với chỉ số đã chọn, ranh giới đối diện ít nhất phải lớn bằng chiều cao đang được xử lý, do đó hệ số giới hạn duy nhất chưa biết là mức chạy tối đa ở phía được xử lý. Điều này ngăn chặn bất kỳ sự điều chỉnh nào sau này làm thay đổi mực nước tính toán. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    out = []

    for _ in range(t):
        n = int(input())
        h = list(map(int, input().split()))

        l, r = 0, n - 1
        left_max, right_max = 0, 0
        water = 0

        while l < r:
            if h[l] <= h[r]:
                if h[l] >= left_max:
                    left_max = h[l]
                else:
                    water += left_max - h[l]
                l += 1
            else:
                if h[r] >= right_max:
                    right_max = h[r]
                else:
                    water += right_max - h[r]
                r -= 1

        out.append(str(water))

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Mã duy trì hai con trỏ co lại về phía trung tâm. Ở mỗi bước, trước tiên, nó sẽ xử lý ranh giới nhỏ hơn vì mực nước của bên đó đã được xác định đầy đủ bởi ranh giới tốt nhất từng thấy cho đến nay. Giá trị cực đại đang chạy đảm bảo chúng tôi không bao giờ tính toán lại các lần quét trên mảng và dữ liệu tích lũy`water`biến thu thập các khoản đóng góp ngay lập tức. 

Một lỗi thực hiện phổ biến là so sánh`h[l]`Và`h[r]`nhưng quên duy trì các cực đại riêng biệt, dẫn đến việc đếm thiếu khi các đỉnh bên trong xuất hiện. Một vấn đề khác là cập nhật con trỏ trước khi sử dụng chiều cao hiện tại, điều này làm thay đổi logic theo một chỉ mục và âm thầm làm hỏng kết quả. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
h = [0, 1, 0, 2, 1, 0]
```| tôi | r | h[l] | h[r] | trái_max | đúng_max | hành động | nước | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 0 | 5 | 0 | 0 | 0 | 0 | quá trình l | 0 | 
| 1 | 5 | 1 | 0 | 1 | 0 | quá trình r | 0 | 
| 1 | 4 | 1 | 1 | 1 | 1 | quá trình r | 0 | 
| 1 | 3 | 1 | 2 | 1 | 1 | quá trình l | 0 | 
| 2 | 3 | 0 | 2 | 1 | 1 | quá trình l | 1 | 
| 3 | 3 | - | - | - | - | dừng lại | 1 | 

Dấu vết này cho thấy nước chỉ được thêm vào khi một vị trí nằm hoàn toàn dưới ranh giới được duy trì. Thuật toán không bao giờ đánh giá lại các vị trí trước đó, xác nhận thuộc tính một lần. 

### Ví dụ 2 

đầu vào:```
h = [3, 0, 2, 0, 4]
```| tôi | r | h[l] | h[r] | trái_max | đúng_max | hành động | nước | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 0 | 4 | 3 | 4 | 3 | 0 | quá trình l | 0 | 
| 1 | 4 | 0 | 4 | 3 | 0 | quá trình l | 3 | 
| 2 | 4 | 2 | 4 | 3 | 0 | quá trình l | 3 | 
| 3 | 4 | 0 | 4 | 3 | 0 | quá trình l | 6 | 

Ví dụ thứ hai nêu bật cách các thung lũng thấp lặp đi lặp lại tích tụ nước hoàn toàn dựa vào ranh giới bên trái cho đến khi phía bên phải trở nên phù hợp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) trên mỗi trường hợp thử nghiệm, O(tổng n) tổng thể | mỗi chỉ mục được xử lý một lần bằng cách di chuyển con trỏ | 
| Không gian | O(1) | chỉ một số biến đang chạy được sử dụng | 

Tổng kích thước đầu vào chỉ là 2×10⁴, do đó, quét tuyến tính trên tất cả các phần tử dễ dàng phù hợp trong giới hạn thời gian. Việc sử dụng bộ nhớ không đổi bất kể kích thước đầu vào, đây là mức tối ưu cho vấn đề này. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    def solve():
        t = int(input())
        out = []
        for _ in range(t):
            n = int(input())
            h = list(map(int, input().split()))
            l, r = 0, n - 1
            left_max, right_max = 0, 0
            water = 0
            while l < r:
                if h[l] <= h[r]:
                    if h[l] >= left_max:
                        left_max = h[l]
                    else:
                        water += left_max - h[l]
                    l += 1
                else:
                    if h[r] >= right_max:
                        right_max = h[r]
                    else:
                        water += right_max - h[r]
                    r -= 1
            out.append(str(water))
        return "\n".join(out)

    return solve()

# provided sample (implicit from statement formatting)
assert run("1\n12\n0 1 0 2 1 0 1 3 2 1 2 1\n") == "6"

# minimum size
assert run("1\n1\n5\n") == "0"

# all equal
assert run("1\n4\n2 2 2 2\n") == "0"

# increasing
assert run("1\n5\n1 2 3 4 5\n") == "0"

# decreasing
assert run("1\n5\n5 4 3 2 1\n") == "0"

# valley case
assert run("1\n5\n3 0 3 0 3\n") == "6"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 0 | trường hợp cơ sở đúng đắn | 
| tất cả đều bình đẳng | 0 | không đặt bẫy giả trên địa hình bằng phẳng | 
| ngày càng tăng | 0 | không hình thành ranh giới bên trái | 
| giảm dần | 0 | không hình thành ranh giới bên phải | 
| thung lũng đối xứng | 6 | tích lũy chính xác trên nhiều hố | 

## Vỏ cạnh 

Đối với một yếu tố đầu vào như`[7]`, thuật toán đặt cả hai con trỏ ở cùng một chỉ mục và kết thúc ngay lập tức mà không cần xử lý, tạo ra 0. Không có ranh giới nào được hình thành nên không thể đếm được nước. 

Đối với một cấu trúc phẳng như`[4, 4, 4, 4]`, cả hai`left_max`Và`right_max`được cập nhật ngay lập tức lên 4 và mọi so sánh đều không tìm thấy khoảng cách nào dưới ranh giới. Nước tích lũy luôn bằng 0, phù hợp với diễn giải vật lý dự kiến. 

Đối với dãy tăng dần, phía con trỏ bên trái luôn cập nhật`left_max`ở mỗi bước trước khi phát hiện bất kỳ sự thiếu hụt nào. Vì không có phần tử nào ở dưới mức tối đa nên không có nước được thêm vào. Logic tương tự được áp dụng đối xứng cho các chuỗi giảm nghiêm ngặt, trong đó con trỏ bên phải chiếm ưu thế và ngăn cản sự tích lũy ở phía đó.
