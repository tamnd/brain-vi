---
title: "CF 104333G - Hình vuông song song có trục bao quanh tối thiểu"
description: "Chúng ta được cấp một số tập hợp điểm độc lập trên mặt phẳng 2D. Đối với mỗi bộ, chúng ta phải đặt tất cả các điểm bên trong một hình vuông thẳng hàng với trục, nghĩa là các cạnh của hình vuông song song với các trục tọa độ."
date: "2026-07-01T18:56:53+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104333
codeforces_index: "G"
codeforces_contest_name: "Replay of BU - PSTU Programming club collaborative contest"
rating: 0
weight: 104333
solve_time_s: 78
verified: false
draft: false
---

[CF 104333G - Hình vuông song song có trục bao quanh tối thiểu](https://codeforces.com/problemset/problem/104333/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 18s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một số tập hợp điểm độc lập trên mặt phẳng 2D. Đối với mỗi bộ, chúng ta phải đặt tất cả các điểm bên trong một hình vuông thẳng hàng với trục, nghĩa là các cạnh của hình vuông song song với các trục tọa độ. Trong số tất cả các hình vuông chứa đầy đủ mọi điểm như vậy, chúng ta muốn hình vuông có diện tích nhỏ nhất có thể. 

Đối với một ca kiểm thử cố định, ý nghĩa hình học rất đơn giản: chúng ta được phép đặt một hình vuông ở bất cứ đâu, nhưng độ dài cạnh của nó phải đủ lớn để bao phủ toàn bộ trải rộng của các điểm theo cả hai hướng x và y. Một khi chúng ta biết chiều dài cạnh thì diện tích chính là hình vuông của nó. 

Các ràng buộc đẩy chúng ta tới một giải pháp logarit hoặc hằng số trên mỗi thử nghiệm. Tổng số điểm trên tất cả các trường hợp thử nghiệm tối đa là 200.000, do đó, bất kỳ giải pháp nào xử lý từng điểm với số lần không đổi đều có thể chấp nhận được. Bất cứ điều gì bậc hai cho mỗi trường hợp thử nghiệm sẽ ngay lập tức thất bại, vì ngay cả sự phân chia đầu vào vừa phải cũng sẽ vượt quá giới hạn thời gian. 

Một vấn đề tế nhị phát sinh từ phạm vi tọa độ. Tọa độ có thể lớn tới ±10^9, do đó, bất kỳ cách tiếp cận nào dựa vào việc liệt kê các vị trí vuông góc ứng viên hoặc hình học cưỡng bức trên mặt phẳng đều không khả thi. 

Các trường hợp cạnh chính đến từ các tập điểm suy biến. Nếu tất cả các điểm giống hệt nhau thì hình vuông bao quanh phải có cạnh dài 0, tạo ra diện tích bằng 0. Nếu chỉ có một điểm, logic tương tự sẽ được áp dụng. Một trường hợp khác là khi các điểm được căn chỉnh thành một đường theo chiều ngang hoặc chiều dọc, trong đó chỉ có một chiều xác định câu trả lời. Một sai lầm ngây thơ là tính diện tích như`(max_x - min_x) * (max_y - min_y)`mà không thực thi ràng buộc bình phương, điều này đánh giá thấp cạnh bình phương cần thiết khi một chiều lớn hơn. 

Ví dụ, điểm`(0,0)`Và`(10,1)`cho chiều rộng là 10 và chiều cao là 1. Diện tích hình chữ nhật sẽ là 10, nhưng hình vuông phải có cạnh là 10 thì diện tích là 100. 

## Phương pháp tiếp cận 

Ý tưởng mạnh mẽ là xem xét mọi vị trí hình vuông có thể. Người ta có thể tưởng tượng việc chọn một góc dưới bên trái và độ dài cạnh, sau đó kiểm tra xem tất cả các điểm có vừa với bên trong hay không. Đối với mỗi ứng viên, chúng tôi quét tất cả các điểm, dẫn đến xác minh O(n). Nếu chúng ta rời rạc hóa các vị trí ứng cử viên bằng cách sử dụng tọa độ điểm, chúng ta vẫn có khả năng O(n^2) hoặc tệ hơn đối với tâm hoặc góc, vì bất kỳ điểm nào cũng có thể xác định ranh giới. Điều này nhanh chóng trở nên không thể vượt quá n khoảng vài nghìn. 

Quan sát quan trọng là một hình vuông thẳng hàng với trục bao quanh tất cả các điểm được xác định hoàn toàn bởi tọa độ cực trị. Hình chữ nhật nhỏ nhất chứa tất cả các điểm có chiều rộng`max_x - min_x`và chiều cao`max_y - min_y`. Bất kỳ hình vuông nào cũng phải có độ dài cạnh ít nhất bằng giá trị lớn hơn trong hai giá trị này, vì việc thu nhỏ một trong hai chiều sẽ loại trừ ít nhất một điểm cực trị. 

Một khi chúng ta chấp nhận rằng độ dài cạnh được cố định bởi các cực trị này thì hình vuông tối ưu chỉ đơn giản là hình vuông tối thiểu có cạnh bằng`max(max_x - min_x, max_y - min_y)`. Chúng ta không cần cân nhắc lựa chọn vị trí ngoài việc đảm bảo hình vuông có thể được đặt ở bất cứ đâu; việc dịch chuyển hình vuông không làm thay đổi độ dài cạnh yêu cầu. 

Do đó, mỗi trường hợp thử nghiệm giảm xuống mức tính toán quét tuyến tính tối thiểu và tối đa cho x và y. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) hoặc tệ hơn | O(1) | Quá chậm | 
| Tối ưu | O(n) mỗi lần kiểm tra | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đối với mỗi trường hợp kiểm thử, đọc tất cả các điểm và theo dõi bốn giá trị: x tối thiểu, x tối đa, y tối thiểu, y tối đa. Bốn giá trị này nắm bắt toàn bộ phạm vi hình học của tập hợp mà không lưu trữ cấu trúc trung gian. 
2. Tính nhịp ngang là`dx = max_x - min_x`. Điều này thể hiện chiều rộng nhỏ nhất có thể có của bất kỳ hình chữ nhật thẳng hàng theo trục nào chứa tất cả các điểm. 
3. Tính nhịp dọc như`dy = max_y - min_y`. Điều này thể hiện chiều cao nhỏ nhất có thể có của bất kỳ hình chữ nhật nào như vậy. 
4. Xác định độ dài cạnh hình vuông cần tìm là`side = max(dx, dy)`. Bước này thực thi ràng buộc hình vuông bằng cách đảm bảo cả hai kích thước đều vừa với một cạnh có độ dài bằng nhau. 
5. Tính diện tích như`side * side`và xuất nó cho trường hợp thử nghiệm. 

Lý do chính khiến bước 4 đúng là vì bất kỳ hình vuông nào cũng phải đồng thời bao phủ cả khoảng trải ngang và chiều dọc. Nếu cạnh nhỏ hơn dx hoặc dy thì ít nhất một cặp điểm cực trị sẽ nằm bên ngoài hình vuông theo chiều đó. 

### Tại sao nó hoạt động 

Tập hợp các điểm xác định một hình chữ nhật giới hạn tối thiểu được căn chỉnh theo các trục. Bất kỳ hình vuông thẳng hàng nào chứa tất cả các điểm đều phải chứa đầy đủ hình chữ nhật này. Do đó, chiều dài cạnh của nó không thể nhỏ hơn cạnh lớn hơn của hình chữ nhật. Ngược lại, một hình vuông có cạnh`max(dx, dy)`luôn có thể được định vị để che hình chữ nhật giới hạn, vì chúng ta không bị ràng buộc ở một điểm gốc cố định. Điều này làm cho phía tính toán vừa cần vừa đủ, đảm bảo tính tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        min_x = 10**18
        max_x = -10**18
        min_y = 10**18
        max_y = -10**18

        for _ in range(n):
            x, y = map(int, input().split())
            if x < min_x:
                min_x = x
            if x > max_x:
                max_x = x
            if y < min_y:
                min_y = y
            if y > max_y:
                max_y = y

        dx = max_x - min_x
        dy = max_y - min_y
        side = max(dx, dy)
        out.append(str(side * side))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Việc triển khai trực tiếp duy trì cực trị đang chạy cho x và y trong khi đọc đầu vào, tránh lưu trữ danh sách điểm. Điều này rất quan trọng vì tổng số điểm trên tất cả các trường hợp thử nghiệm có thể lên tới 200.000. 

Sự tinh tế duy nhất là khởi tạo các giá trị tối thiểu và tối đa. Việc sử dụng các giá trị trọng điểm lớn cũng đảm bảo tính chính xác cho tọa độ âm. Phép tính cạnh bình phương cuối cùng chỉ sử dụng số học số nguyên, điều này an toàn vì các chênh lệch nằm trong phạm vi 64 bit. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1
2
0 0
10 1
```Chúng tôi theo dõi cực trị như sau: 

| Bước | phút_x | max_x | phút_y | max_y | dx | nhuộm | bên | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| Sau (0,0) | 0 | 0 | 0 | 0 | - | - | - | 
| Sau (10,1) | 0 | 10 | 0 | 1 | 10 | 1 | 10 | 

Cạnh cuối cùng là 10 nên diện tích là 100. Điều này khẳng định rằng dù chiều dọc nhỏ nhưng hình vuông phải giãn nở để phù hợp với nhịp ngang. 

### Ví dụ 2 

đầu vào:```
1
3
-2 0
0 2
2 0
```| Bước | phút_x | max_x | phút_y | max_y | dx | nhuộm | bên | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| Sau (-2,0) | -2 | -2 | 0 | 0 | - | - | - | 
| Sau (0,2) | -2 | 0 | 0 | 2 | 2 | 2 | 2 | 
| Sau (2,0) | -2 | 2 | 0 | 2 | 4 | 2 | 4 | 

Cạnh cuối cùng là 4, do đó diện tích là 16. Trường hợp này cho thấy mức chênh lệch đối xứng trong đó phạm vi ngang chiếm ưu thế sau khi xử lý tất cả các điểm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi điểm được xử lý một lần để cập nhật extrema | 
| Không gian | O(1) | Chỉ có bốn biến được duy trì bất kể kích thước đầu vào | 

Tổng thời gian cho tất cả các trường hợp thử nghiệm là O(2·10^5), vừa vặn trong giới hạn. Việc sử dụng bộ nhớ không đổi ngoài việc đệm đầu vào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import isclose

    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        min_x = 10**18
        max_x = -10**18
        min_y = 10**18
        max_y = -10**18

        for _ in range(n):
            x, y = map(int, input().split())
            min_x = min(min_x, x)
            max_x = max(max_x, x)
            min_y = min(min_y, y)
            max_y = max(max_y, y)

        side = max(max_x - min_x, max_y - min_y)
        out.append(str(side * side))

    return "\n".join(out)

# provided sample
assert run("""2
2
0 0
1 0
2
0 0
0 4
""") == "1\n16"

# single point
assert run("""1
1
5 5
""") == "0"

# all points same line horizontal
assert run("""1
3
0 0
5 0
10 0
""") == "100"

# all points same line vertical
assert run("""1
3
2 1
2 5
2 9
""") == "64"

# mixed negative coordinates
assert run("""1
2
-3 -3
1 2
""") == "25"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| điểm duy nhất | 0 | bình phương suy biến diện tích 0 | 
| đường ngang | 100 | chiều rộng thống trị chiều cao | 
| đường thẳng đứng | 64 | chiều cao chiếm ưu thế chiều rộng | 
| âm bản hỗn hợp | 25 | xử lý đúng giới hạn âm | 

## Vỏ cạnh 

Đối với một điểm duy nhất như`(5, 5)`, thuật toán đặt tất cả các cực trị bằng nhau, tạo ra`dx = dy = 0`và do đó diện tích`0`. Điều này phù hợp với thực tế là hình vuông có cạnh bằng 0 vẫn chứa điểm. 

Đối với các điểm được căn chỉnh theo chiều ngang như`(0,0), (5,0), (10,0)`, nhịp dọc bằng 0 trong khi nhịp ngang là 10. Thuật toán tính toán`side = 10`, tạo ra diện tích 100. Mọi nỗ lực sử dụng diện tích hình chữ nhật sẽ xuất sai 0 cho đóng góp chiều cao hoặc 0 cho diện tích nếu xử lý sai. 

Đối với các điểm được căn chỉnh theo chiều dọc như`(2,1), (2,5), (2,9)`, khoảng ngang bằng 0 và khoảng dọc là 8. Thuật toán trả về cạnh 8 và diện tích 64, mở rộng chính xác theo chiều ngang ngay cả khi không tồn tại dải ngang. 

Đối với tọa độ âm hỗn hợp như`(-3,-3)`Và`(1,2)`, việc xử lý cực trị đảm bảo sự khác biệt chính xác bất kể dấu hiệu. Các nhịp tính toán là`dx = 4`,`dy = 5`, cho cạnh 5 và diện tích 25. Điều này xác nhận rằng việc khởi tạo và cập nhật tối thiểu/tối đa xử lý chính xác toàn bộ phạm vi số nguyên.
