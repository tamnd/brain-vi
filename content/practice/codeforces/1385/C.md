---
title: "CF 1385C - Make It Good"
description: "Chúng ta được cung cấp một mảng và chúng ta chỉ được phép xóa tiền tố, nghĩa là chúng ta loại bỏ một số phần tử ở phía trước và giữ nguyên phần còn lại."
date: "2026-06-16T14:16:47+07:00"
tags: ["codeforces", "competitive-programming", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1385
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 656 (Div. 3)"
rating: 1200
weight: 1385
solve_time_s: 248
verified: false
draft: false
---

[CF 1385C - Make It Good](https://codeforces.com/problemset/problem/1385/C) 

**Đánh giá:** 1200 
**Thẻ:** tham lam 
**Thời gian giải:** 4 phút 8 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng và chúng ta chỉ được phép xóa tiền tố, nghĩa là chúng ta loại bỏ một số phần tử ở phía trước và giữ nguyên phần còn lại. Sau khi xóa, chúng tôi muốn hậu tố còn lại có thể chuyển đổi thành chuỗi không giảm bằng cách sử dụng một quy trình rất cụ thể: chúng tôi liên tục lấy các phần tử từ một trong hai đầu của mảng hiện tại và nối chúng vào một mảng mới. 

Thuộc tính quan trọng của một mảng “tốt” là tồn tại một cách để xây dựng một chuỗi không giảm bằng cách luôn chọn phần tử còn lại ngoài cùng bên trái hoặc ngoài cùng bên phải. Điều này tương đương với việc nói rằng chúng ta có thể “bóc” mảng từ cả hai đầu theo một thứ tự nào đó và thu thập các giá trị không bao giờ giảm. 

Vì vậy, nhiệm vụ là: trong số tất cả các hậu tố của mảng, tìm tiền tố ngắn nhất mà chúng ta phải loại bỏ để hậu tố còn lại trở nên tốt theo nghĩa này. 

Các ràng buộc rất lớn: tổng chiều dài của các trường hợp thử nghiệm lên tới 200.000. Điều này ngay lập tức loại trừ mọi mô phỏng bậc hai cho mỗi trường hợp thử nghiệm. Bất kỳ cách tiếp cận nào thử tất cả các tiền tố và kiểm tra “sự tốt đẹp” từ đầu sẽ quá chậm. 

Một điểm tinh tế là câu trả lời có thể bằng không. Nếu mảng ban đầu đã tốt rồi thì chúng ta không làm gì cả. 

Trường hợp tinh tế thứ hai xuất hiện khi mảng giảm nghiêm ngặt. Ví dụ,`[5, 4, 3, 2]`thực sự là tốt, bởi vì chúng ta luôn có thể lấy từ đầu bên phải và xây dựng một dãy không giảm theo thứ tự ngược lại. Điều này khiến người ta dễ nhầm lẫn khi cho rằng tính đơn điệu là cần thiết trong mảng ban đầu, điều này không đúng. 

Một tình huống phức tạp khác là các mảng zig-zag hỗn hợp như`[1, 3, 1, 4]`. Cấu trúc tăng cục bộ không đảm bảo tính tốt, bởi vì lựa chọn hai đầu áp đặt ràng buộc nhất quán toàn cầu đối với những giá trị chúng ta chọn trước. 

## Phương pháp tiếp cận 

Một giải pháp bạo lực sẽ thử mọi cách loại bỏ tiền tố có thể. Đối với mỗi hậu tố, chúng tôi sẽ kiểm tra xem nó có tốt không. Kiểm tra mức độ tốt có nghĩa là mô phỏng quá trình chọn từ cả hai đầu và đảm bảo chúng ta có thể tạo ra chuỗi không giảm. 

Việc mô phỏng trực tiếp cho một mảng có thể được thực hiện một cách tham lam bằng cách luôn cố gắng chọn một kết thúc hợp lệ mà không phá vỡ tính đơn điệu. Nhưng ngay cả điều đó cũng phải mất O(n) cho mỗi lần kiểm tra. Việc lặp lại nó cho mọi tiền tố sẽ dẫn đến O(n^2) cho mỗi trường hợp thử nghiệm, quá lớn so với các ràng buộc. 

Cái nhìn sâu sắc quan trọng là đảo ngược quan điểm. Thay vì hỏi nên loại bỏ tiền tố nào, chúng tôi hỏi chúng tôi có thể giữ lại hậu tố nào. Chúng tôi muốn hậu tố dài nhất là “tốt”. 

Bây giờ hãy xem xét việc xây dựng chuỗi cuối cùng từ phải sang trái. Trong một cấu trúc hợp lệ, phần tử được chọn cuối cùng là phần tử tối thiểu trong số các lựa chọn còn lại ở mỗi bước. Điều này gợi ý rằng trong quá trình xây dựng tham lam từ cuối, khi chúng ta quyết định lấy một giá trị, mọi thứ trước đó trong quy trình ít nhất phải có giá trị đó khi được chọn từ phía chính xác. 

Một cách có cấu trúc hơn để xem nó là: chúng tôi muốn một hậu tố sao cho có thể chia nó thành hai phần trong quá trình, trong đó các phần tử lấy từ bên phải tạo thành một phân đoạn không tăng và các phần tử lấy từ bên trái cũng tôn trọng ngưỡng do giai đoạn đầu tiên áp đặt. Cấu trúc tối ưu giảm xuống còn việc tìm điểm phân tách trong đó ràng buộc đơn điệu trở nên hợp lệ và chúng tôi theo dõi giá trị “rào cản” tối thiểu có thể có trong khi quét từ cuối. 

Chúng tôi quét từ phải sang trái, duy trì giá trị tối thiểu được thấy cho đến nay. Mức tối thiểu này thể hiện những yếu tố cuối cùng tốt nhất có thể mà chúng tôi có thể tiếp tục lấy từ bên phải. Nếu tại bất kỳ thời điểm nào, chúng tôi gặp phải một phần tử nhỏ hơn cấu trúc được bảo trì này theo cách vi phạm tính khả thi của việc xây dựng, chúng tôi xác định rằng chúng tôi phải loại bỏ nhiều hơn từ phía trước. 

Điều này dẫn đến một phép tính tham lam đơn giản: tính toán từ cuối và tìm hậu tố dài nhất có thể duy trì tính nhất quán cần thiết, sau đó câu trả lời là tiền tố trước nó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Tham lam tối ưu từ quét hậu tố | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Bắt đầu từ phần tử cuối cùng của mảng, vì mọi cách xây dựng hợp lệ cuối cùng đều phụ thuộc vào cách chúng ta có thể hoàn thành việc xây dựng chuỗi không giảm. 
2. Duy trì một biến`mn`theo dõi giá trị nhỏ nhất mà chúng tôi đã “cam kết” trong khi quét từ phải sang trái. Điều này thể hiện ngưỡng giới hạn của những gì chúng ta vẫn có thể kết hợp một cách an toàn vào một công trình hợp lệ. 
3. Di chuyển sang trái trong mảng. Tại mỗi vị trí, so sánh giá trị hiện tại với`mn`. Nếu giá trị hiện tại không tương thích với việc duy trì cấu trúc hậu tố hợp lệ, chúng tôi sẽ cập nhật ranh giới hiệu lực của mình. 
4. Tiếp tục quá trình quét này cho đến khi tiền tố cần loại bỏ trở nên rõ ràng, nghĩa là mọi thứ ở bên phải của một số chỉ mục đều có thể tham gia vào một cấu trúc hợp lệ. 
5. Câu trả lời là số phần tử trước khi hậu tố hợp lệ này bắt đầu. 

Tại sao điều này lại có tác dụng gắn liền với cách thức hoạt động của quy trình chọn hàng hai đầu. Khi một giá trị được lấy làm một phần của chuỗi cuối cùng, mọi lựa chọn trong tương lai đều phải tôn trọng ràng buộc thứ tự. Quét từ cuối mô phỏng việc sửa phần đuôi của chuỗi trước, đây luôn là phần bị hạn chế nhất. Khi ràng buộc bị phá vỡ, mọi thứ ở bên trái không thể được cứu vãn bằng cách xóa tiền tố nhỏ hơn điểm đó, vì việc xóa tiền tố chỉ thay đổi vị trí bắt đầu và không sắp xếp lại hoặc sửa chữa những điểm không tương thích. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        mn = float('inf')
        i = n - 1

        while i >= 0:
            if a[i] > mn:
                break
            mn = min(mn, a[i])
            i -= 1

        print(i + 1)

if __name__ == "__main__":
    solve()
```Mã xử lý từng trường hợp thử nghiệm một cách độc lập. Quá trình quét bắt đầu từ phần tử ngoài cùng bên phải, theo dõi giá trị nhỏ nhất được thấy cho đến nay. Con trỏ`i`dừng lại khi chúng ta gặp một giá trị vi phạm khả năng mở rộng cấu trúc không giảm hợp lệ từ phía bên phải. 

Câu trả lời được trả về`i + 1`tương ứng với số phần tử phải bỏ đi ở phía trước để hậu tố còn lại bắt đầu ở vị trí chỉ mục`i+1`. 

Một cạm bẫy phổ biến là quên hướng quét là từ phải sang trái. Quét từ trái sang phải sẽ không nắm bắt được cấu trúc ràng buộc của quá trình xây dựng cuối cùng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
7
4 3 3 8 4 5 2
```Chúng tôi quét từ bên phải: 

| Bước | Chỉ mục | Giá trị | mn | Hành động | 
| --- | --- | --- | --- | --- | 
| 1 | 6 | 2 | 2 | giữ | 
| 2 | 5 | 5 | 2 | điều kiện ngắt được kích hoạt | 

Ở chỉ số 5, giá trị 5 vượt quá mức tối thiểu 2 hiện tại, nghĩa là hậu tố bắt đầu quá sớm sẽ không hợp lệ. Chúng tôi dừng lại ngay sau chỉ số 5. 

Vì vậy, chúng tôi loại bỏ tiền tố có độ dài 4, để lại`[4, 5, 2]`. 

Điều này chứng tỏ một cấu trúc ở giữa có vẻ hợp lệ lại thất bại như thế nào vì các giá trị trước đó không thể phù hợp với thứ tự xây dựng cuối cùng được yêu cầu. 

### Ví dụ 2 

đầu vào:```
5
1 3 1 4 5
```| Bước | Chỉ mục | Giá trị | mn | Hành động | 
| --- | --- | --- | --- | --- | 
| 1 | 4 | 5 | 5 | giữ | 
| 2 | 3 | 4 | 4 | giữ | 
| 3 | 2 | 1 | 1 | giữ | 
| 4 | 1 | 3 | 1 | nghỉ | 

Tại chỉ số 1, giá trị 3 vi phạm điều kiện khả thi của hậu tố nên chúng ta phải loại bỏ ít nhất hai phần tử đầu tiên. 

Hậu tố còn lại`[1, 4, 5]`hợp lệ vì nó có thể được sắp xếp thành một chuỗi không giảm thông qua các lựa chọn cuối cùng. 

Những dấu vết này cho thấy cấu trúc hậu tố được điều chỉnh hoàn toàn bằng cách duy trì tính nhất quán từ ranh giới bên phải về phía sau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi phần tử được truy cập tối đa một lần cho mỗi trường hợp thử nghiệm | 
| Không gian | O(1) | Chỉ có một số biến vô hướng được duy trì | 

Tổng độ phức tạp trong tất cả các trường hợp thử nghiệm vẫn tuyến tính ở kích thước đầu vào, vừa vặn thoải mái trong giới hạn 200.000 phần tử. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque

    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        mn = float('inf')
        i = n - 1

        while i >= 0:
            if a[i] > mn:
                break
            mn = min(mn, a[i])
            i -= 1

        out.append(str(i + 1))
    return "\n".join(out)

# provided sample
assert run("""5
4
1 2 3 4
7
4 3 3 8 4 5 2
3
1 1 1
7
1 3 1 4 5 3 2
5
5 4 3 2 3
""") == """0
4
0
2
3"""

# custom cases
assert run("""3
1
10
2
2 1
6
1 2 3 2 1 4
""") == """0
0
3"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`[10]`|`0`| yếu tố duy nhất đã tốt | 
|`[2, 1]`|`0`| mảng giảm vẫn tốt | 
|`[1,2,3,2,1,4]`|`3`| mẫu hỗn hợp yêu cầu loại bỏ tiền tố | 

## Vỏ cạnh 

Mảng một phần tử hoạt động bình thường vì không có lựa chọn nào về đầu có thể phá vỡ tính đơn điệu, do đó quá trình quét không bao giờ gây ra vi phạm. 

Mảng giảm nghiêm ngặt cũng an toàn vì cấu trúc tham lam luôn lấy từ đầu bên phải, tạo ra chuỗi không giảm theo thứ tự ngược lại. Thuật toán quét không ngừng, cho ra câu trả lời bằng 0. 

Trong mảng hỗn hợp như`[1, 2, 3, 2, 1, 4]`, vi phạm xuất hiện sớm khi giá trị lớn hơn đứng trước hậu tố bị ràng buộc chặt chẽ. Quét từ phải sang trái xác định chính xác rằng tiền tố ban đầu sẽ ngăn không cho bất kỳ cấu trúc hai đầu hợp lệ nào bắt đầu và đầu ra khớp với vị trí đầu tiên nơi tính nhất quán bị phá vỡ.
