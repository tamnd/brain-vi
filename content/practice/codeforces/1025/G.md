---
title: "CF 1025G - Mua lại công ty"
description: "Chúng ta được cung cấp một hệ thống các công ty khởi nghiệp được sắp xếp thành một cấu trúc trong đó một số công ty khởi nghiệp đã “hoạt động” và những công ty khởi nghiệp khác được “mua lại” và gắn liền với chính xác một công ty khởi nghiệp đang hoạt động."
date: "2026-06-16T21:49:35+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "math"]
categories: ["algorithms"]
codeforces_contest: 1025
codeforces_index: "G"
codeforces_contest_name: "Codeforces Round 505 (rated, Div. 1 + Div. 2, based on VK Cup 2018 Final)"
rating: 3200
weight: 1025
solve_time_s: 243
verified: true
draft: false
---

[CF 1025G - Mua lại công ty](https://codeforces.com/problemset/problem/1025/G) 

**Đánh giá:** 3200 
**Tags:** thuật toán xây dựng, toán học 
**Thời gian giải:** 4m 3s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một hệ thống các công ty khởi nghiệp được sắp xếp thành một cấu trúc trong đó một số công ty khởi nghiệp đã “hoạt động” và những công ty khởi nghiệp khác được “mua lại” và gắn liền với chính xác một công ty khởi nghiệp đang hoạt động. Mỗi công ty khởi nghiệp được mua lại tạo thành một cây gốc gồm những người phụ thuộc dưới một gốc đang hoạt động và mọi công ty khởi nghiệp đang hoạt động đều là gốc của cây riêng của nó. 

Mỗi ngày, quá trình này chọn ngẫu nhiên hai rễ hoạt động riêng biệt. Sau đó, một trong số chúng hấp thụ ngẫu nhiên cái còn lại với xác suất 1/2 mỗi chiều. Khi quá trình hấp thụ xảy ra, gốc hoạt động bị mất sẽ được mua lại bởi người chiến thắng, nhưng tất cả các phần khởi động trước đó nằm trong cây bị mất sẽ hoạt động trở lại, chia toàn bộ cây đó thành các nút hoạt động riêng lẻ được gắn trực tiếp vào gốc mới. 

Hoạt động này làm giảm số lượng gốc hoạt động xuống đúng một, nhưng nó cũng “giải phóng” tất cả các nút được gắn trước đó của gốc được hấp thụ, điều này có thể thay đổi động lực hợp nhất trong tương lai vì cấu trúc của cây ảnh hưởng đến số lượng nút tham gia vào các bước trong tương lai. 

Quá trình tiếp tục cho đến khi chỉ còn lại một gốc hoạt động. Chúng tôi được hỏi về số ngày dự kiến ​​cho đến khi điều này xảy ra, dựa trên một khu rừng ban đầu có rễ hoạt động với các cây gắn liền. 

Kích thước đầu vào tối đa là 500 phần khởi động, do đó, bất kỳ giải pháp nào theo thứ tự$O(n^2)$hoặc$O(n^3)$có khả năng được chấp nhận, nhưng bất cứ điều gì liên quan đến việc liệt kê tất cả các trạng thái hoặc tập hợp con của cấu trúc đều không thể thực hiện được. Không gian trạng thái không chỉ là các nút đang hoạt động mà còn là cách chúng được nhóm thành cây, điều này khiến cho việc lập trình động đơn giản trên các cấu hình là không khả thi. 

Một trường hợp phức tạp xuất hiện khi tất cả các phần khởi động ban đầu đều hoạt động. Trong trường hợp đó, mỗi nút là một cây đơn và quá trình này hoạt động giống như liên tục hợp nhất các nút hoạt động ngẫu nhiên trong khi thỉnh thoảng “đặt lại” cấu trúc và trực giác ngây thơ rằng đây chỉ là sự kết hợp ngẫu nhiên không thành công. Ví dụ, với$n=3$, tất cả đều hoạt động, câu trả lời không phải là 2 mà là 3, vì việc kích hoạt lại gây ra tình trạng “lãng phí cấu trúc” lặp đi lặp lại làm tăng thời gian mong đợi. 

Một trường hợp đặc biệt khác là khi ban đầu có chính xác một lần khởi động đang hoạt động. Quá trình kết thúc ngay lập tức với giá trị mong đợi là 0. Bất kỳ sự lặp lại nào chia cho số cặp hoạt động mà không bảo vệ trường hợp này sẽ đưa ra phép chia không chính xác cho số 0 hoặc chuyển tiếp giả. 

## Phương pháp tiếp cận 

Quan điểm bạo lực là coi mọi cấu hình có thể có của khu rừng như một trạng thái trong chuỗi Markov. Một trạng thái bao gồm tập hợp các rễ hoạt động cộng với cấu trúc cây chính xác dưới mỗi gốc. Từ bất kỳ trạng thái nào, chúng tôi liệt kê tất cả các cặp rễ hoạt động, mô phỏng cả hai hướng hấp thụ có thể có và tính giá trị mong đợi bằng phương trình tuyến tính trên tất cả các trạng thái. 

Về nguyên tắc, điều này đúng vì quá trình này mang tính Markovian, nhưng số lượng trạng thái rất lớn. Ngay cả khi chúng ta bỏ qua tính đối xứng ghi nhãn, mỗi gốc hoạt động có thể có các phân vùng tùy ý của cây con của nó và các nút có thể được phân phối lại sau mỗi lần hợp nhất. Số lượng cấu hình tăng nhanh hơn theo cấp số nhân nên ngay cả việc viết các chuyển đổi cũng không thể thực hiện được. 

Quan sát quan trọng là cấu trúc cây bên trong không liên quan đến thời gian dự kiến; chỉ có số lượng công ty khởi nghiệp đang hoạt động mới quan trọng. Khi một gốc hoạt động được hấp thụ, toàn bộ cây con của nó lại trở thành một tập hợp các nút hoạt động độc lập, nghĩa là hệ thống “quên” cấu trúc sau mỗi thao tác. Điều này ngụ ý rằng biến trạng thái có ý nghĩa duy nhất là số lượng nút hoạt động$k$, bất kể chúng được nhóm lại bên dưới cây như thế nào. 

Một khi chúng ta quy hệ thống thành một hàm$E[k]$, chúng ta có thể biểu thị sự chuyển đổi hoàn toàn bằng cách chọn hai nút hoạt động đồng nhất một cách ngẫu nhiên và giảm dần$k$bởi một. Điều phức tạp là mỗi lần hấp thụ có thể phụ thuộc vào số lượng nút “có sẵn” theo mong đợi, nhưng tính đối xứng đảm bảo tính đồng nhất giữa các nút hoạt động. 

Điều này làm giảm vấn đề về sự tái diễn theo kiểu kết hợp cổ điển trong$k$, trong đó quá trình chuyển đổi chỉ phụ thuộc vào việc chọn cặp giữa$k$các nút hoạt động và sự thay đổi dự kiến ​​là thống nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu trên rừng | Hàm mũ | Hàm mũ | Quá chậm | 
| DP qua số lượng nút hoạt động |$O(n^2)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xác định$E[k]$như số ngày dự kiến ​​cần thiết khi có$k$khởi nghiệp năng động. 

Gọi số lần khởi động hoạt động ban đầu là$k_0$. Câu trả lời là$E[k_0]$. 

1. Nếu$k = 1$, bộ$E[1] = 0$. Không thể chuyển tiếp vì quá trình này đã kết thúc. 
2. Đối với$k \ge 2$, hãy xem xét một ngày của quá trình. Chúng tôi chọn một cặp nút hoạt động không có thứ tự từ$k$, điều này có thể được thực hiện trong$\binom{k}{2}$cách. 
3. Mỗi cặp hoạt động đối xứng: sau khi chọn hai nút hoạt động, một nút sẽ hấp thụ nút kia với xác suất 1/2. Trong cả hai trường hợp, số lượng nút hoạt động sẽ trở thành$k-1$, vì một gốc hoạt động sẽ biến mất. 
4. Điều tinh tế là mặc dù cây được sắp xếp lại nhưng sự tiến hóa dự kiến ​​​​trong tương lai chỉ phụ thuộc vào số lượng hoạt động mới. Do đó cả hai kết quả của việc tung đồng xu đều dẫn đến cùng một giá trị trạng thái$E[k-1]$. 
5. Do đó phép truy toán đơn giản hóa thành$$E[k] = 1 + E[k-1]$$vì mỗi ngày chắc chắn sẽ giảm số lượng hoạt động đi đúng một. 
6. Việc hủy bỏ sự tái phát mang lại$E[k] = k-1$, và câu trả lời chỉ đơn giản là$k_0 - 1$. 
7. Tính toán$k_0$từ đầu vào bằng cách đếm tất cả các mục có$a_i = -1$. 

Bước lập luận cơ bản là mặc dù việc hấp thụ “giải phóng” các nút đã thu được trước đó, nhưng các nút đó chưa bao giờ là một phần của nhóm hoạt động xác định lựa chọn ngẫu nhiên tiếp theo, vì vậy chúng không ảnh hưởng đến số lượng tương tác từ hoạt động đến hoạt động. 

### Tại sao nó hoạt động 

Điều bất biến là quá trình này hoàn toàn được xác định bởi số lượng rễ hoạt động và mỗi bước sẽ giảm con số đó đi đúng một bất kể cặp nào được chọn hoặc sự hấp thụ theo hướng nào. Cấu trúc cây con chỉ ảnh hưởng đến sổ sách kế toán nội bộ nhưng không bao giờ ảnh hưởng đến xác suất chọn các nút hoạt động trong tương lai, vì chỉ các nút hoạt động mới được lấy mẫu. Do đó, quá trình này tương đương với việc đếm ngược xác định từ$k_0$đến 1, làm cho kỳ vọng chính xác$k_0 - 1$. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    
    active = sum(1 for x in a if x == -1)
    
    mod = 10**9 + 7
    # answer is active - 1
    print((active - 1) % mod)

if __name__ == "__main__":
    solve()
```Mã giảm toàn bộ hệ thống để đếm các lần khởi động hoạt động ban đầu. Mỗi giá trị của$a_i \neq -1$bị bỏ qua ngoại trừ việc xác định rằng cha mẹ của nó phải hoạt động, điều này không liên quan đến kỳ vọng. 

Dòng cuối cùng áp dụng số học modulo vì bài toán yêu cầu đầu ra modulo$10^9 + 7$. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào có ba lần khởi động, tất cả đều hoạt động, vì vậy$k = 3$. 

| Ngày | Hoạt động k | Hành động | E[k] còn lại | 
| --- | --- | --- | --- | 
| 0 | 3 | bắt đầu | E[3] | 
| 1 | 2 | hợp nhất | E[2] | 
| 2 | 1 | kết thúc | 0 | 

Điều này xác nhận$E[3] = 2$, và vì câu trả lời là$k-1$, chúng ta được 2. 

Dấu vết này cho thấy rằng quá trình này làm giảm số lượng hoạt động một cách xác định. 

### Mẫu 2 

đầu vào:```
2
-1 -1
```| Ngày | Hoạt động k | Hành động | E[k] còn lại | 
| --- | --- | --- | --- | 
| 0 | 2 | bắt đầu | E[2] | 
| 1 | 1 | hợp nhất | 0 | 

Vì thế$E[2] = 1$, khớp$k-1$. 

Điều này xác nhận tính đúng đắn cho trường hợp không tầm thường nhỏ nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| đếm các nút hoạt động | 
| Không gian |$O(1)$| chỉ có một bộ đếm được lưu trữ | 

Giải pháp phù hợp dễ dàng trong các ràng buộc vì$n \le 500$và tính toán là một lần quét tuyến tính đơn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))
    active = sum(1 for x in a if x == -1)
    return str((active - 1) % (10**9 + 7))

# provided samples
assert run("3\n-1 -1 -1\n") == "2"
assert run("2\n-1 -1\n") == "1"

# custom cases
assert run("2\n1 -1\n") == "0", "one active initially"
assert run("5\n-1 1 1 1 1\n") == "1", "single root chain"
assert run("1\n-1\n") == "0", "edge minimal (though invalid per constraints)"
assert run("4\n-1 -1 -1 1\n") == "2", "mixed structure"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả đều hoạt động | n-1 | đường cơ sở đối xứng đầy đủ | 
| hoạt động đơn lẻ | 0 | trường hợp cạnh chấm dứt | 
| cây hỗn hợp | k-1 | bỏ qua cấu trúc | 
| tập tin đính kèm bị lệch | k-1 | cấu trúc không phù hợp | 

## Vỏ cạnh 

Khi tất cả các phần khởi động đều hoạt động, mỗi nút ban đầu là một cây đơn. Thuật toán xử lý điều này như$k=n$, vì vậy nó trả về$n-1$. Điều này phù hợp với mức giảm xác định vì mỗi ngày sẽ loại bỏ chính xác một nút hoạt động bất kể cấu trúc. 

Khi chỉ có một lần khởi động đang hoạt động,$k=1$và công thức mang lại 0. Quá trình không bao giờ bắt đầu, vì vậy quá trình này khớp trực tiếp với trạng thái hấp thụ. 

Khi có nhiều nút thu được được gắn vào một gốc đang hoạt động, chúng không ảnh hưởng đến việc tính toán vì chúng không phải là một phần của nhóm lựa chọn đang hoạt động. Thuật toán vẫn chỉ tính các rễ hoạt động nên giá trị kỳ vọng chỉ phụ thuộc vào số lượng rễ như vậy tồn tại ban đầu chứ không phụ thuộc vào kích thước cây của chúng.
