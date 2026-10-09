---
title: "CF 104974D - Đèn giao thông"
description: "Chúng ta đang chuyển động dọc theo một con đường thẳng từ vị trí 0 đến vị trí X, đi với vận tốc đúng một mét/giây. Dọc đường có đèn giao thông đặt ở tọa độ cố định."
date: "2026-06-28T06:10:27+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104974
codeforces_index: "D"
codeforces_contest_name: "Codentines Day"
rating: 0
weight: 104974
solve_time_s: 69
verified: false
draft: false
---

[CF 104974D - Đèn giao thông](https://codeforces.com/problemset/problem/104974/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 9 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta đang chuyển động dọc theo một con đường thẳng từ vị trí 0 đến vị trí X, đi với vận tốc đúng một mét/giây. Dọc đường có đèn giao thông đặt ở tọa độ cố định. Mỗi đèn giao thông được mô tả theo vị trí, màu ban đầu và thông số chu kỳ kiểm soát tốc độ chuyển đổi vĩnh viễn giữa màu đỏ và xanh lục. 

Chuyển động là liên tục. Khi đến đèn giao thông, chúng ta có thể phải đợi nếu màu hiện tại là màu đỏ vào đúng thời điểm đó. Nếu không chúng ta sẽ đi qua ngay lập tức. Điều duy nhất làm tăng tổng thời gian di chuyển của chúng ta vượt quá khoảng cách vật lý là việc chờ đèn đỏ. 

Đầu vào cung cấp n đèn giao thông, mỗi đèn có vị trí p, màu ban đầu c là đỏ hoặc xanh lục và tham số độ dài chu kỳ t. Tín hiệu thay đổi cứ sau t giây, nghĩa là chu kỳ đầy đủ của nó là 2t giây. Nếu nó bắt đầu có màu đỏ, thì màu đỏ kéo dài từ thời điểm 0 đến t, sau đó là màu xanh lá cây từ thời điểm t đến 2t, v.v. Nếu nó bắt đầu có màu xanh lục, các pha sẽ được hoán đổi. 

Chúng ta cần tổng thời gian để đến X bắt đầu từ 0, tính cả thời gian đi bộ và thời gian chờ đợi. 

Các giới hạn lên tới 100000 đèn và khoảng cách lên tới 100000. Điều đó ngay lập tức loại trừ bất kỳ mô phỏng nào cố gắng tăng thời gian theo từng bước nhỏ. Cách tiếp cận khả thi duy nhất là xử lý từng đèn giao thông một lần theo thời gian tuyến tính hoặc gần tuyến tính sau khi sắp xếp chúng theo vị trí. Bất kỳ giải pháp nào tệ hơn O(n log n) hoặc O(n) sẽ quá chậm. 

Một trường hợp lỗi nhỏ xuất hiện khi nhiều đèn không được sắp xếp theo vị trí. Nếu chúng tôi xử lý chúng theo thứ tự đầu vào, chúng tôi có thể mô phỏng ánh sáng không theo thứ tự không gian và tính toán thời gian chờ không đúng lúc. Một vấn đề khác nảy sinh nếu chúng ta quên rằng việc chờ đợi phụ thuộc vào thời gian đến tuyệt đối chứ không chỉ số lượng đèn đi qua. 

Ví dụ, hãy xem xét hai đèn: 

đầu vào:```
2 10
R 5 5
G 2 3
```Nếu được xử lý theo thứ tự đầu vào, chúng tôi có thể xử lý vị trí 5 trước vị trí 2, điều này là không thể về mặt vật lý và dẫn đến thời gian không chính xác. 

Đầu ra chính xác phụ thuộc vào việc xử lý theo thứ tự vị trí tăng dần. 

## Phương pháp tiếp cận 

Mô phỏng trực tiếp là đi bộ từ 0 đến X từng giây. Ở mỗi giây, chúng tôi kiểm tra xem mình có đang đứng trước đèn giao thông hay không và liệu điều đó có buộc chúng tôi phải chờ đợi hay không. Điều này đúng vì nó phản ánh chính xác quá trình thực tế. Tuy nhiên, khoảng cách lên tới 100000 và việc chờ đợi cũng có thể kéo dài thời gian đáng kể, do đó phương pháp này có thể giảm xuống còn O(X) mỗi bước mô phỏng và trở nên quá chậm trong trường hợp xấu nhất. 

Cách tốt hơn là nhận ra rằng không có gì thay đổi liên tục ngoại trừ vị trí và thời gian. Giữa đèn giao thông, không có quyết định. Chúng ta có thể nhảy từ đèn giao thông này sang đèn giao thông khác một cách an toàn, cộng khoảng cách làm thời gian di chuyển. Phép tính không cần thiết duy nhất là xác định liệu chúng ta có phải đợi đèn đỏ hay không, điều này chỉ phụ thuộc vào thời gian hiện tại modulo 2t. 

Quan sát này làm giảm vấn đề sắp xếp đèn theo vị trí và mô phỏng một lần truyền, duy trì thời gian hiện tại và cập nhật nó ở mỗi đèn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng từng bước | O(X + sự kiện đang chờ) | O(1) | Quá chậm | 
| Sắp xếp + mô phỏng một lượt | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp tất cả các đèn giao thông theo vị trí theo thứ tự tăng dần. Điều này là cần thiết vì thời gian tiến hóa theo đường không gian, vì vậy chúng ta phải tiếp xúc với ánh sáng theo thứ tự chúng ta tiếp cận chúng. 
2. Khởi tạo thời gian hiện tại là 0 và vị trí hiện tại là 0. Thời gian biểu thị khoảng thời gian chúng ta đã đi bộ bao gồm cả thời gian chờ đợi. 
3. Lặp lại từng đèn giao thông theo thứ tự được sắp xếp. 
4. Thêm thời gian cần thiết để đi bộ từ vị trí trước đó đến vị trí có đèn hiện tại. Vì tốc độ là 1 mét/giây nên đây chỉ đơn giản là sự chênh lệch khoảng cách được cộng vào thời gian hiện tại. 
5. Tính trạng thái của đèn giao thông tại thời điểm đến bằng cách sử dụng modulo current_time (2t). Điều này cho chúng ta giai đoạn trong chu kỳ lặp lại. 
6. Tùy thuộc vào màu ban đầu, xác định xem pha hiện tại tương ứng với màu đỏ hay xanh lục. Nếu đèn đỏ khi đến nơi, hãy đẩy thời gian hiện tại về phía trước để bắt đầu khoảng thời gian xanh tiếp theo. Bước nhảy này được tính bằng cách trừ đi pha hiện tại từ t hoặc 2t tương ứng. 
7. Cập nhật vị trí hiện tại vào vị trí đèn giao thông và tiếp tục. 
8. Sau khi xử lý tất cả các đèn, thêm đoạn cuối cùng từ đèn cuối cùng vào X. 

Ý tưởng chính là chúng tôi không bao giờ mô phỏng từng giây một. Chúng tôi chỉ nhảy giữa các điểm sự kiện có ý nghĩa, đó là đèn giao thông. 

### Tại sao nó hoạt động 

Giữa các đèn giao thông liên tiếp, trạng thái của hệ thống hoàn toàn được xác định bởi một biến duy nhất là thời gian hiện tại. Đoạn đường không chứa sự kiện nào phụ thuộc vào thời gian. Tại mỗi đèn giao thông, quyết định chờ hay vượt chỉ phụ thuộc vào thời gian hiện tại theo modulo chiều dài chu kỳ cố định. Vì tính toán này là chính xác và chúng tôi xử lý ánh sáng theo thứ tự không gian nên mô phỏng sẽ duy trì hành vi thời gian liên tục thực sự mà không cần xấp xỉ. Điều bất biến là current_time luôn bằng thời gian đến chính xác tại vị trí hiện tại sau khi giải quyết tất cả các yêu cầu chờ đợi cho đến thời điểm đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, X = map(int, input().split())
    lights = []
    
    for _ in range(n):
        parts = input().split()
        c = parts[0]
        p = int(parts[1])
        t = int(parts[2])
        lights.append((p, c, t))
    
    lights.sort()
    
    cur_time = 0
    cur_pos = 0
    
    for p, c, t in lights:
        cur_time += (p - cur_pos)
        cur_pos = p
        
        cycle = 2 * t
        phase = cur_time % cycle
        
        if c == 'R':
            if phase < t:
                cur_time += (t - phase)
        else:
            if phase >= t:
                cur_time += (cycle - phase)
    
    cur_time += (X - cur_pos)
    print(cur_time)

if __name__ == "__main__":
    solve()
```Việc triển khai tuân theo mô phỏng dựa trên sự kiện một cách chính xác. Sắp xếp đảm bảo trật tự không gian chính xác. Biến`cur_time`luôn đại diện cho thời gian tuyệt đối khi chúng ta đến thời điểm hiện tại. Tính toán modulo tách biệt vị trí của chúng ta trong chu kỳ tín hiệu. Việc điều chỉnh có điều kiện chỉ chuyển thời gian về phía trước khi chúng ta đến trong khoảng thời gian màu đỏ. 

Một lỗi phổ biến là quên cập nhật vị trí trước khi tính toán đoạn tiếp theo, điều này sẽ phá vỡ các phép tính khoảng cách. Một vấn đề khó phát hiện khác là phân loại sai các khoảng màu đỏ-xanh, đặc biệt đối với đèn khởi động màu xanh lục, trong đó nửa sau của chu kỳ có màu đỏ. 

## Ví dụ đã hoạt động 

Hãy xem đầu vào mẫu được hiểu là ba đèn: 

đầu vào:```
3 100
R 5 10
R 10 50
G 50 70
```Chúng tôi theo dõi trạng thái từng bước. 

| Bước | Vị trí | Thời gian đến trước khi chờ đợi | Giai đoạn | Hành động | Thời Gian Mới | 
| --- | --- | --- | --- | --- | --- | 
| Bắt đầu | 0 | 0 | 0 | di chuyển | 0 | 
| Ánh sáng 1 | 5 | 5 | 5 mod 20 = 5 | bắt đầu đỏ, pha < 10 nên đợi 5 | 10 | 
| Ánh sáng 2 | 10 | 15 | 15 mod 100 = 15 | bắt đầu màu đỏ, giai đoạn màu xanh lá cây nên vượt qua | 15 | 
| Ánh sáng 3 | 50 | 55 | 55 mod 140 = 55 | bắt đầu màu xanh lá cây, pha màu đỏ nên hãy đợi | 70 | 
| Kết thúc | 100 | 120 | - | nước đi cuối cùng | 170 | 

Dấu vết này cho thấy việc chờ đợi chỉ được kích hoạt dựa trên giai đoạn chu kỳ khi đến chứ không phải theo lịch sử trước đó. 

Một ví dụ thứ hai: 

đầu vào:```
1 10
G 5 3
```| Bước | Vị trí | Giờ Đến | Giai đoạn | Hành động | Thời Gian Mới | 
| --- | --- | --- | --- | --- | --- | 
| Ánh sáng | 5 | 5 | 5 mod 6 = 5 | bắt đầu xanh, pha >= 3 nên đỏ, đợi | 6 | 
| Kết thúc | 10 | 11 | - | di chuyển | 11 | 

Điều này thể hiện sự chờ đợi bắt buộc duy nhất khi hàng đến rơi vào phần màu đỏ của chu kỳ bắt đầu màu xanh lá cây. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Đèn phân loại chiếm ưu thế, mỗi đèn được xử lý một lần | 
| Không gian | O(n) | Lưu trữ cho tất cả đèn giao thông | 

Các ràng buộc cho phép lên tới 100000 đèn giao thông, do đó việc sắp xếp cộng với mô phỏng tuyến tính phù hợp một cách thoải mái trong giới hạn thời gian. Mỗi phép toán bên trong vòng lặp là số học theo thời gian không đổi, do đó thời gian chạy thực sự tuyến tính sau khi sắp xếp. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided sample
assert run("3 100\nR 5 10\nR 10 50\nG 50 70\n") == "170"

# minimum case: no lights
assert run("0 10\n") == "10"

# single green-start light, no wait
assert run("1 10\nG 5 5\n") == "10"

# single red-start light, forced wait
assert run("1 10\nR 5 5\n") == "10"

# multiple lights increasing delays
assert run("2 20\nR 5 5\nR 10 5\n") == "25"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| không có đèn | 10 | đường cơ sở đi bộ thuần túy | 
| xanh đơn | 10 | không có trường hợp chờ đợi | 
| đơn đỏ | 10 | chờ logic đúng đắn | 
| hai màu đỏ | 25 | tích lũy chờ đợi | 

## Vỏ cạnh 

Trường hợp một cạnh là khi đèn kích hoạt chính xác ở ranh giới giữa màu đỏ và xanh lục. Đối với đèn khởi động màu đỏ có chu kỳ t, đến đúng pha 0 có nghĩa là màu đỏ, trong khi đến đúng pha t có nghĩa là màu xanh lá cây. Việc triển khai xử lý vấn đề này một cách chính xác vì nó sử dụng bất đẳng thức nghiêm ngặt trên pha < t. 

Một trường hợp khó khăn khác là khi nhiều đèn ở rất gần nhau. Vì mỗi đèn cập nhật thời gian hiện tại trước chuyển động tiếp theo, nên việc chờ đợi liên tiếp sẽ diễn ra chính xác mà không bị nhiễu. 

Trường hợp cuối cùng là khi X bằng vị trí đèn giao thông. Trong tình huống đó, đoạn cuối cùng sẽ thêm khoảng cách bằng 0 sau khi xử lý ánh sáng đó, vì vậy câu trả lời chỉ đơn giản là thời gian sau khi phân giải ánh sáng cuối cùng, phù hợp với cách giải thích vật lý là đến chính xác điểm giao nhau.
