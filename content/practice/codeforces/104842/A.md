---
title: "CF 104842A - Cuộc phiêu lưu ở vùng đất bằng phẳng"
description: "Chúng ta được cho hai điểm trên một lưới số nguyên. Cả điểm bắt đầu và điểm đến đều nằm cách xa trục tọa độ, nghĩa là cả hai điểm cuối đều không có tọa độ nào bằng 0. Những điểm như vậy được gọi là điểm miễn phí."
date: "2026-06-28T11:31:39+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104842
codeforces_index: "A"
codeforces_contest_name: "2020-2021 ICPC, Moscow Subregional"
rating: 0
weight: 104842
solve_time_s: 47
verified: true
draft: false
---

[CF 104842A - Cuộc phiêu lưu ở vùng đất bằng phẳng](https://codeforces.com/problemset/problem/104842/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 47s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho hai điểm trên một lưới số nguyên. Cả điểm bắt đầu và điểm đến đều nằm cách xa trục tọa độ, nghĩa là cả hai điểm cuối đều không có tọa độ nào bằng 0. Những điểm như vậy được gọi là điểm miễn phí. Lưới cũng chứa các điểm “đắt”, chính xác là các điểm nằm trên trục, trong đó tọa độ x hoặc tọa độ y bằng 0. Mỗi khi một đường đi qua bất kỳ điểm trục nào, chi phí sẽ là 1. 

Nhiệm vụ là di chuyển từ điểm bắt đầu đến đích bằng cách sử dụng bất kỳ đường dẫn liên tục nào trong mặt phẳng, không bị giới hạn ở các cạnh lưới hoặc các đoạn thẳng. Điều duy nhất quan trọng là có bao nhiêu điểm trục mà đường dẫn chạm tới ít nhất một lần. Mục tiêu là giảm thiểu con số này. 

Khó khăn chính là mặt phẳng liên tục nên đường đi có thể bị uốn cong tùy ý. Tuy nhiên, cấu trúc chi phí là rời rạc và chỉ phụ thuộc vào việc đường đi cắt trục x hay trục y và nó giao nhau bao nhiêu lần theo cách không thể tránh được do biến dạng. 

Giới hạn tọa độ đủ nhỏ để bất kỳ lý do O(1) nào cho mỗi bài kiểm tra đều đủ. Với các giá trị có độ lớn lên tới 10.000, bất kỳ giải pháp nào cố gắng mô phỏng hình học hoặc rời rạc hóa mặt phẳng đều không cần thiết và sẽ là quá mức cần thiết. Lời giải đúng phải đến từ suy luận hình học về số lượng trục phải cắt nhau theo cấu trúc liên kết. 

Một sự hiểu lầm ngây thơ sẽ là cho rằng mỗi khi đường đi qua x = 0 hoặc y = 0 thì nó lại được trả tiền. Điều đó sẽ dẫn đến việc đếm quá mức, vì một đường dẫn được chọn tốt có thể đi qua mỗi trục nhiều nhất một lần trong cấu hình tối ưu. 

Lỗi phổ biến thứ hai là giả sử lý luận kiểu Manhattan, chẳng hạn như tính tổng các thay đổi tọa độ tuyệt đối hoặc xem xét các bước lưới. Điều này không chính xác vì đường đi liên tục và có thể cắt ngang các góc phần tư một cách tự do. 

Các trường hợp nguy hiểm phá vỡ những ý tưởng ngây thơ bao gồm: 

Điểm bắt đầu và điểm kết thúc trong cùng một góc phần tư. Ví dụ: (2, 3) đến (5, 7). Một cách tiếp cận xuyên trục ngây thơ có thể vẫn cho rằng bạn phải vượt qua một trục, nhưng bạn có thể hoàn toàn ở trong cùng một góc phần tư và không phải trả tiền. 

Điểm bắt đầu và điểm kết thúc trong các góc phần tư đối diện theo đường chéo, chẳng hạn như (1, 1) đến (-1, -1). Ở đây cả hai tọa độ đều đổi dấu, buộc một đường đi cắt cả hai trục và do đó phải chịu ít nhất 2 chi phí. 

Trường hợp hỗn hợp như (1, 1) đến (-1, 2), trong đó chỉ có dấu x thay đổi, do đó chỉ có một trục cắt nhau là không thể tránh khỏi. 

Nhiệm vụ thực sự giảm xuống còn việc suy luận về những thay đổi dấu trong tọa độ và liệu có thể tránh được các trục hoàn toàn bằng cách định tuyến qua một góc phần tư hay liệu việc giao nhau có bị ép buộc về mặt cấu trúc hay không. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ sẽ cố gắng xem xét các đường dẫn một cách rõ ràng. Người ta có thể tưởng tượng việc rời rạc hóa mặt phẳng thành một lưới mịn và thực hiện tìm kiếm đường đi ngắn nhất trong đó việc đi qua một trục sẽ làm tăng chi phí. Điều này sẽ mô hình hóa vấn đề một cách chính xác vì chi phí sẽ tăng thêm trên các giao điểm trục. Tuy nhiên, biểu đồ là vô hạn và liên tục, và bất kỳ sự rời rạc nào đủ tốt để nắm bắt tất cả các hành vi hợp lệ sẽ bùng nổ về kích thước. Ngay cả việc giới hạn trong một lưới giới hạn có kích thước 20.000 x 20.000 cũng dẫn đến hàng trăm triệu nút, khiến BFS hoặc Dijkstra không khả thi. 

Nhận xét quan trọng là mặt phẳng không thực sự phức tạp trong bài toán này. Cấu trúc có ý nghĩa duy nhất là sự phân chia thành bốn góc phần tư cách nhau bởi các trục. Bên trong một góc phần tư, sự chuyển động được tự do. Chi phí thời gian duy nhất phát sinh là khi đi qua x = 0 hoặc y = 0. Hơn nữa, mỗi trục có thể được cắt qua nhiều nhất một lần trên một đường đi tối ưu, vì việc đi qua lại một trục sẽ chỉ tăng thêm chi phí không cần thiết mà không cải thiện khả năng tiếp cận.

Điều này làm giảm vấn đề thành một câu hỏi tổ hợp thuần túy về việc liệu điểm bắt đầu và điểm kết thúc có nằm trong cùng một góc phần tư, chia sẻ một mẫu dấu tọa độ hay đối diện nhau theo đường chéo. Nếu cả hai tọa độ có cùng dấu ở cả hai điểm cuối, chúng ta không bao giờ cần phải cắt một trục, do đó chi phí bằng 0. Nếu có đúng một dấu tọa độ khác nhau thì chúng ta phải cắt chính xác một trục. Nếu cả hai đều khác nhau, chúng ta phải cắt cả hai trục, mang lại chi phí là hai. 

Do đó, hình học thu gọn lại thành so sánh dấu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (tìm kiếm dạng lưới) | O(N²) hoặc tệ hơn | O(N2) | Quá chậm | 
| Phân tích dấu hiệu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta biến đổi mỗi điểm thành một cặp dấu, một cho x và một cho y. Mỗi dấu hiệu cho chúng ta biết điểm nằm ở phía nào của trục tương ứng. 

1. Đọc tọa độ điểm bắt đầu và điểm đích. Thông tin duy nhất chúng ta thực sự cần là mỗi tọa độ là dương hay âm, vì số 0 không bao giờ xuất hiện trong đầu vào. 
2. Xác định xem x1 và x2 có cùng dấu hay không. Nếu chúng khác nhau thì việc di chuyển từ đầu đến cuối phải cắt qua trục y ít nhất một lần. Điều này là do việc thay đổi dấu của x đòi hỏi phải đi qua x = 0. 
3. Xác định xem y1 và y2 có cùng dấu hay không. Nếu chúng khác nhau, việc di chuyển từ đầu đến cuối phải đi qua trục x ít nhất một lần, vì việc đổi dấu của y đòi hỏi phải đi qua y = 0. 
4. Đếm xem hai dấu hiệu so sánh này khác nhau bao nhiêu. Số lượng đó là số lần cắt trục tối thiểu được yêu cầu. 
5. Xuất số đếm làm câu trả lời. 

Logic hoạt động vì mỗi trục tương ứng với một rào cản tôpô. Việc thay đổi dấu trên một tọa độ không thể xảy ra nếu không cắt trục tương ứng của nó và mỗi trục giao nhau phải chịu chính xác một chi phí. 

### Tại sao nó hoạt động 

Mặt phẳng được chia thành bốn góc phần tư mở bởi các trục. Mọi đường đi liên tục giữa hai điểm phải liên tục trong không gian được phân vùng này. Di chuyển giữa các góc phần tư tương ứng chính xác với việc đi qua một trong các trục. Vì chi phí chỉ phát sinh khi chạm vào các điểm trục nên chi phí tối thiểu chính xác là số trục riêng biệt ngăn cách góc phần tư bắt đầu và kết thúc. Không có đường dẫn nào có thể giảm con số này vì việc tránh cắt trục có nghĩa là ở trong vùng được kết nối không chứa đích. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def sign(x):
    return 1 if x > 0 else -1

def solve():
    x1, y1, x2, y2 = map(int, input().split())

    dx = sign(x1) != sign(x2)
    dy = sign(y1) != sign(y2)

    print(dx + dy)

if __name__ == "__main__":
    solve()
```Giải pháp giảm từng tọa độ về dấu của nó bằng cách sử dụng hàm trợ giúp. Vì số 0 được đảm bảo không xuất hiện nên chúng ta chỉ phân biệt giá trị dương và giá trị âm. 

Sau đó chúng ta so sánh dấu của tọa độ x và tọa độ y một cách độc lập. Mỗi sự không khớp góp phần tạo ra một trục giao nhau không thể tránh khỏi. Câu trả lời cuối cùng là tổng của những sự không phù hợp này. 

Việc thực hiện là thời gian không đổi và tránh mọi mô phỏng hình học. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
25 11 -20 -20
```Chúng tôi chỉ theo dõi các dấu hiệu. 

| Bước | ký hiệu x1 | ký hiệu x2 | ký hiệu y1 | ký hiệu y2 | x không khớp | y không khớp | trả lời | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| ban đầu | + | - | + | - | 1 | 1 | 2 | 

Điểm bắt đầu nằm ở góc phần tư thứ nhất và đích đến nằm ở góc phần tư thứ ba. Cả hai tọa độ đều đổi dấu nên cả hai trục phải cắt nhau. Kết quả là 2. 

### Ví dụ 2 

đầu vào:```
3 5 7 9
```| Bước | ký hiệu x1 | ký hiệu x2 | ký hiệu y1 | ký hiệu y2 | x không khớp | y không khớp | trả lời | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| ban đầu | + | + | + | + | 0 | 0 | 0 | 

Cả hai điểm đều nằm trong cùng một góc phần tư. Một đường dẫn có thể vẫn nằm hoàn toàn bên trong góc phần tư đó mà không cần chạm vào một trong hai trục. Chi phí bằng không. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ kiểm tra và so sánh dấu hiệu theo thời gian cố định cho mỗi lần kiểm tra | 
| Không gian | O(1) | Không sử dụng cấu trúc phụ trợ | 

Giải pháp này thỏa mãn một cách tầm thường các ràng buộc vì nó chỉ thực hiện một số phép tính số học và so sánh bất kể độ lớn tọa độ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    def sign(x):
        return 1 if x > 0 else -1

    x1, y1, x2, y2 = map(int, input().split())
    dx = sign(x1) != sign(x2)
    dy = sign(y1) != sign(y2)
    return str(dx + dy)

# provided sample
assert run("25 11 -20 -20\n") == "2", "sample 1"

# same quadrant
assert run("1 2 3 4\n") == "0"

# only x changes sign
assert run("1 2 -3 4\n") == "1"

# only y changes sign
assert run("1 2 3 -4\n") == "1"

# both change sign
assert run("1 2 -3 -4\n") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 2 3 4 | 0 | cùng một góc phần tư, chi phí bằng 0 | 
| 1 2 -3 4 | 1 | chéo trục đơn | 
| 1 2 -3 -4 | 2 | chuyển động của góc phần tư đối diện | 

## Vỏ cạnh 

Một trường hợp tinh vi là khi cả hai điểm đều nằm trong cùng một góc phần tư. Ví dụ: (10, 5) đến (3, 7). Thuật toán tính toán các dấu giống hệt nhau cho cả hai tọa độ, dẫn đến điểm giao nhau bằng 0. Đường đi có thể được vẽ hoàn toàn trong góc phần tư đó mà không cần tiếp cận một trong hai trục, do đó không phát sinh chi phí. 

Một trường hợp khác là khi chỉ có một tọa độ đổi dấu, chẳng hạn như (5, 10) thành (-2, 8). Dấu x khác nhau trong khi dấu y khớp. Thuật toán trả về 1, tương ứng với một giao điểm bắt buộc của trục y. Bất kỳ đường đi liên tục nào cũng phải đi qua x = 0 tại một điểm nào đó để lật dấu x. 

Cuối cùng, khi cả hai tọa độ đều đổi dấu, chẳng hạn như (2, 3) thành (-4, -5), cả hai điểm không khớp đều được tính. Đường đi phải đi qua cả hai trục theo một thứ tự nào đó và không có biến dạng nào có thể tránh được cả hai điểm giao nhau vì điểm bắt đầu và điểm kết thúc nằm trong các góc phần tư đối diện theo đường chéo.
