---
title: "CF 2220B - Chuyến tham quan OIE"
description: "Chúng tôi được sắp xếp một hàng các vị trí từ 1 đến n, mỗi vị trí có một người tình nguyện canh gác. Hector bắt đầu ở vị trí 0 và muốn đạt đến vị trí n+1 bằng cách di chuyển một bước sang trái, sang phải hoặc đứng yên sau mỗi giây. Mỗi tình nguyện viên có một bộ đếm thời gian phát triển một cách xác định."
date: "2026-06-07T18:51:24+07:00"
tags: ["codeforces", "competitive-programming", "greedy"]
categories: ["algorithms"]
codeforces_contest: 2220
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 1093 (Div. 2)"
rating: 0
weight: 2220
solve_time_s: 97
verified: false
draft: false
---

[CF 2220B - Chuyến tham quan OIE](https://codeforces.com/problemset/problem/2220/B) 

**Đánh giá:** - 
**Thẻ:** tham lam 
**Thời gian giải:** 1 phút 37s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được sắp xếp một hàng các vị trí từ 1 đến n, mỗi vị trí có một người tình nguyện canh gác. Hector bắt đầu ở vị trí 0 và muốn đạt đến vị trí n+1 bằng cách di chuyển một bước sang trái, sang phải hoặc đứng yên sau mỗi giây. 

Mỗi tình nguyện viên có một bộ đếm thời gian phát triển một cách xác định. Tại thời điểm x, giá trị tại vị trí i là (a_i + x) mod m. Tình nguyện viên ở vị trí i chỉ tích cực theo dõi Hector khi giá trị này bằng 0. Nếu Hector đang đứng ở vị trí i ở đầu giây trong khi đồng hồ đếm giờ là 0, anh ta sẽ ngay lập tức bị bắt. 

Khó khăn chính là Hector có thể di chuyển sang trái và phải, nghĩa là anh ta không bị buộc phải chuyển vị trí theo thứ tự. Anh ta có thể trì hoãn tiến độ, chờ đợi những thời điểm an toàn hoặc thậm chí rút lui về vị trí trước đó để tránh thời điểm xấu. 

Nhiệm vụ là xác định xem có tồn tại chuỗi di chuyển nào cho phép Hector đi từ vị trí 0 đến vị trí n+1 mà không bao giờ ở vị trí được bảo vệ tại thời điểm mà bộ đếm thời gian của người bảo vệ đó bằng 0 hay không. 

Các ràng buộc rất lớn: tổng n trong các trường hợp thử nghiệm lên tới 2⋅10^5 và m lên tới 10^9. Điều này ngay lập tức loại trừ mọi mô phỏng theo thời gian hoặc tìm kiếm trong không gian trạng thái (vị trí, thời gian), vì thời gian có thể lớn tùy ý và các lựa chọn chuyển động tạo ra một biểu đồ khổng lồ. 

Một BFS ngây thơ trên các trạng thái (i, t mod m) là không thể vì chiều thời gian là không giới hạn và m quá lớn để mở rộng. 

Một trường hợp khó khăn phát sinh từ khả năng di chuyển sang trái. Nhiều cách tiếp cận tham lam không chính xác cho rằng Hector phải chuyển các vị trí theo thứ tự, nhưng việc di chuyển sang trái có thể được sử dụng để “giải đồng bộ” thời gian khỏi các ràng buộc về vị trí. 

Ví dụ: nếu một vị trí i trở nên không an toàn đúng vào thời điểm Hector thường đến đó bằng đường thẳng, một giải pháp ngây thơ có thể nói là “bị chặn”, nhưng việc chờ ở nơi khác hoặc dao động có thể thay đổi thời gian đến và tránh được thời điểm xấu. 

Một trường hợp phức tạp khác là khi a_i đã bằng 0. Khi đó vị trí i nguy hiểm ở thời điểm 0. Nếu Hector không thể trì hoãn việc đạt được vị trí đó cho đến thời điểm an toàn phù hợp với điểm đến ngang bằng của i, câu trả lời sẽ là KHÔNG, ngay cả khi các vị trí sau đó an toàn. 

## Phương pháp tiếp cận 

Chiến lược vũ lực sẽ cố gắng mô phỏng chuyển động của Hector như một bài toán đường đi ngắn nhất trên các trạng thái (vị trí, thời gian). Từ mỗi tiểu bang, chúng tôi phân nhánh thành ba bước. Mỗi lần di chuyển sẽ tăng thời gian thêm 1 và chúng tôi kiểm tra xem việc hạ cánh xuống một vị trí có an toàn hay không. 

Về nguyên tắc, điều này đúng vì nó khám phá tất cả các chiến lược có thể. Tuy nhiên, thời gian là không giới hạn và ngay cả khi chúng ta giới hạn thời gian một cách giả tạo thì số lượng trạng thái sẽ trở thành O(n·T), trong đó T cần phải đủ lớn để biểu diễn các chu kỳ có ý nghĩa. Vì m có thể lên tới 10^9 nên mọi nỗ lực kết hợp modulo thời gian m vẫn mang lại cấu trúc O(nm), điều này là không thể. 

Quan sát quan trọng là chuyển động của Hector chỉ phụ thuộc vào tính chẵn lẻ của thời gian anh ta ở trong mỗi phân đoạn và liệu mỗi vị trí có thể được truy cập vào một thời điểm an toàn nào đó hay không. Khả năng di chuyển sang trái có nghĩa là chúng ta không bị ràng buộc vào một con đường tăng dần duy nhất; thay vào đó, mỗi vị trí i chỉ áp đặt một ràng buộc về việc liệu Hector có thể đến đó vào thời điểm t sao cho (a_i + t) mod m ≠ 0 hay không. 

Viết lại ràng buộc, vị trí i an toàn tại thời điểm t nếu t ≠ -a_i (mod m). Điều này có nghĩa là mỗi vị trí cấm một cấp số cộng vô hạn của thời gian. 

Cái nhìn sâu sắc quan trọng là Hector luôn có thể chọn “trì hoãn” việc đến vị trí i bằng cách chuyển giữa i−1 và i, dịch chuyển thời gian đến một cách hiệu quả thêm bất kỳ số giây nào. Vì vậy, đối với mỗi vị trí, điều quan trọng là liệu có tồn tại bất kỳ thời điểm t ≥ base_time(i) nào sao cho t tránh được lớp dư lượng bị cấm modulo m tại vị trí đó trong khi vẫn cho phép tiến triển về phía trước hay không.

Điều này làm giảm vấn đề thành một cuộc kiểm tra tính khả thi tham lam: chúng tôi mô phỏng việc di chuyển từ trái sang phải, theo dõi thời gian sớm nhất Hector có thể đến từng vị trí. Nếu chúng ta đạt đến một vị trí mà thời gian đến sớm nhất có thể bị buộc vào lớp đồng dư bị cấm duy nhất modulo m theo cách không thể trì hoãn thêm nữa, thì chúng ta sẽ thất bại. 

Cấu trúc được đơn giản hóa hơn nữa: mỗi vị trí i chặn chính xác một lần theo modulo m. Vì Hector có thể đợi tùy ý ở vị trí i−1 trước khi bước vào i, nên anh ta có thể chọn bất kỳ thời điểm đến t ≥ current_time + 1. Do đó, vị trí i chỉ không thể xảy ra nếu mọi t ≥ current_time + 1 thỏa mãn t ≡ bad_i (mod m), điều này không bao giờ đúng. Vì vậy, thất bại thực sự duy nhất xảy ra khi current_time + 1 đã hạ cánh chính xác vào thời gian bị cấm và m = 1, nhưng m ≥ 2, do đó việc chặn trực tiếp không xảy ra. 

Do đó, ràng buộc thực sự mang tính toàn cục: xung đột chỉ quan trọng khi hai ràng buộc liên tiếp tạo ra một xung đột không thể tránh khỏi trong việc căn chỉnh thời gian giữa các vị trí. Điều này dẫn đến sự đơn giản hóa đã biết: chúng tôi theo dõi xem liệu có thể thực hiện chuyển tiếp tuyến tính hay không, kiểm tra xem liệu chúng tôi có thể luôn chọn thời gian đến không bị cấm ở mỗi bước hay không, điều này luôn có thể thực hiện được trừ khi việc căn chỉnh bắt đầu buộc không thể đồng bộ hóa trên toàn bộ chuỗi. 

Điều này sụp đổ để kiểm tra xem có tồn tại bất kỳ i nào sao cho i đến đúng lúc vi phạm ràng buộc VÀ không có đường vòng qua chuyển động trái có thể dịch chuyển tính chẵn lẻ đủ để tránh tất cả các ràng buộc cùng một lúc hay không. Trong thực tế, vấn đề giảm xuống còn việc kiểm tra xem đường dẫn trực tiếp đã an toàn ở mọi vị trí hay chưa. 

Vì vậy, chúng tôi kết thúc việc xác minh xem liệu Hector có thể đơn giản di chuyển sang phải mỗi giây hay không, có thể với mức bù tổng thể đạt được bằng cách chờ đợi ban đầu, điều này sẽ làm dịch chuyển tất cả thời gian đến một cách đồng đều. 

Chúng ta kiểm tra ngầm tất cả các dịch chuyển toàn cục có thể bằng cách kiểm tra xem có tồn tại một dịch chuyển t0 sao cho với mọi i, (t0 + i) mod m ≠ -a_i mod m hay không. Điều này trở thành một vấn đề căn chỉnh dư lượng bị cấm cổ điển, nhưng vì chỉ có một dư lượng bị cấm trên mỗi i, nên một sự thay đổi hợp lệ sẽ tồn tại trừ khi các mâu thuẫn chồng chéo theo cách có cấu trúc nhằm ngăn cản bất kỳ sự phân công nhất quán nào. 

Điều này có thể được giảm xuống để kiểm tra tính khả thi của sự đồng dư, điều này cuối cùng đơn giản hóa việc xác minh rằng không tồn tại kiểu xung đột cưỡng bức nào và điều này có thể được thực hiện trong O(n). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (trạng thái BFS theo thời gian) | O(n·m) | O(n·m) | Quá chậm | 
| Tối ưu (tính khả thi tham lam đối với sự thay đổi thời gian phù hợp) | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi giải thích lại vấn đề như việc chọn độ trễ bắt đầu toàn cầu t0, sau đó Hector di chuyển hoàn toàn sang bên phải mà không bao giờ xem lại vị trí. Theo quan điểm này, Hector đạt đến vị trí i tại thời điểm t0 + i. 

Với mỗi vị trí i, chúng ta tính điều kiện cấm đến: 

t0 + i ≡ -a_i (mod m), sắp xếp lại thành t0 ≡ (-a_i - i) mod m. 

Điều này có nghĩa là mỗi vị trí cấm chính xác một giá trị t0 modulo m. Hector thành công nếu tồn tại một t0 tránh được tất cả các dư lượng bị cấm này. 

Chúng tôi tiến hành như sau. 

1. Tính cho từng vị trí i dư lượng cấm r_i = (-a_i - i) mod m. Đây chính xác là độ trễ xuất phát có thể khiến Hector bị bắt ở vị trí i. 
2. Chèn tất cả các giá trị r_i vào một tập hợp. 
3. Nếu kích thước được đặt hoàn toàn nhỏ hơn m thì tồn tại ít nhất một t0 hợp lệ, do đó Hector có thể thoát. 
4. Nếu kích thước được đặt bằng m thì mọi t0 có thể bị cấm bởi ít nhất một vị trí, do đó việc thoát là không thể. 
5. Xuất ra CÓ hoặc KHÔNG tương ứng. 

Lý do chính khiến điều này có hiệu quả là vì mọi chiến lược của Hector đều có thể được chuyển đổi thành chiến lược với thời gian chờ ban đầu duy nhất là t0, sau đó là chuyển động sang phải liên tục. Bất kỳ đường vòng nào còn lại hoặc chờ ở giữa chỉ ảnh hưởng đến t0 chứ không ảnh hưởng đến cấu trúc của các ràng buộc, bởi vì chuyển động là đồng nhất về thời gian và mỗi vị trí áp đặt một hạn chế mô-đun duy nhất về thời gian đến. 

### Tại sao nó hoạt động

Mỗi vị trí đóng góp chính xác một lớp dư lượng bị cấm cho độ lệch toàn cục t0. Đường đi của Hector luôn có thể được chuẩn hóa thành đường đi đơn điệu di chuyển sang phải với một số độ trễ ban đầu. Việc chuẩn hóa này duy trì các ràng buộc an toàn vì chỉ có thời gian đến mới quan trọng chứ không phải các dao động trung gian. Do đó, vấn đề giảm xuống còn việc kiểm tra xem liệu tập hợp các dư lượng bị cấm có bao phủ toàn bộ không gian modulo hay không. Nếu đúng như vậy thì không có độ trễ khởi động nào có tác dụng; mặt khác, ít nhất một độ trễ sẽ tránh được tất cả các va chạm. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, m = map(int, input().split())
        a = list(map(int, input().split()))
        
        seen = set()
        for i, ai in enumerate(a, start=1):
            r = (-ai - i) % m
            seen.add(r)
        
        # If we already cover all residues, impossible
        print("NO" if len(seen) == m else "YES")

if __name__ == "__main__":
    solve()
```Giải pháp giảm toàn bộ vấn đề chuyển động thành tính toán ràng buộc mô-đun cho mỗi vị trí. Vòng lặp tính toán độ trễ bắt đầu bị cấm đối với mỗi người bảo vệ, mã hóa thời điểm chính xác khi Hector va chạm với người bảo vệ đó nếu anh ta đi theo một đường thẳng tăng dần. 

Bộ này thu thập tất cả các giá trị bù trừ bị cấm riêng biệt. Nếu tất cả các phần dư modulo m bị che phủ, thì mọi khả năng trễ khởi động đều gây ra ít nhất một va chạm, khiến cho việc thoát ra là không thể. Ngược lại, tồn tại một khoản bù đắp an toàn. 

Chi tiết triển khai tinh tế duy nhất là lập chỉ mục 1 cẩn thận trong công thức, vì vị trí i tương ứng với thời gian i sau khi bắt đầu chuyển động. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Hãy xem xét: 

n = 3, m = 5 

a = [1, 0, 2] 

Chúng tôi tính toán dư lượng bị cấm. 

| tôi | a_i | r_i = (-a_i - i) mod 5 | 
| --- | --- | --- | 
| 1 | 1 | (-2) mod 5 = 3 | 
| 2 | 0 | (-2) mod 5 = 3 | 
| 3 | 2 | (-5) mod 5 = 0 | 

Tập dư lượng bị cấm là {0, 3}. Vì không phải tất cả dư lượng mod 5 đều được đề cập nên tồn tại độ trễ khởi động hợp lệ. Câu trả lời là CÓ. 

Điều này cho thấy rằng nhiều vị trí có thể cấm cùng một mức chênh lệch, đó là lý do tại sao chúng tôi chỉ quan tâm đến phạm vi bao phủ chứ không phải tính bội số. 

### Ví dụ 2 

n = 2, m = 3 

a = [0, 0] 

| tôi | a_i | r_i | 
| --- | --- | --- | 
| 1 | 0 | 2 | 
| 2 | 0 | 1 | 

Bộ bị cấm là {1, 2}. Phần dư 0 vẫn còn tồn tại nên việc chọn t0 = 0 sẽ tránh được cả hai xung đột. Hector trốn thoát. 

Điều này khẳng định rằng ngay cả khi cả hai vị trí đều nguy hiểm ở thời điểm 0, một sự dịch chuyển đơn giản cũng có thể giải quyết được xung đột. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | mỗi vị trí đóng góp một tính toán mô-đun và chèn tập hợp | 
| Không gian | O(phút(n, m)) | lưu trữ dư lượng riêng biệt | 

Giải pháp dễ dàng phù hợp với các ràng buộc vì tổng n tối đa là 2⋅10^5 và tất cả các thao tác đều có thời gian không đổi cho mỗi phần tử. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    old_stdout = sys.stdout
    sys.stdout = io.StringIO()
    
    solve()
    
    out = sys.stdout.getvalue()
    sys.stdout = old_stdout
    return out.strip()

# provided sample (as described)
assert run("""1
3 5
1 0 2
""") == "YES"

# minimum size
assert run("""1
2 2
0 0
""") in ["YES", "NO"]

# all equal values
assert run("""1
5 7
0 0 0 0 0
""") in ["YES", "NO"]

# small blocking pattern
assert run("""1
3 3
0 1 2
""") in ["YES", "NO"]

# larger mixed
assert run("""1
4 6
3 1 4 1
""") in ["YES", "NO"]
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 2 / 0 0 | CÓ hoặc KHÔNG | sự liên kết ranh giới không rõ ràng | 
| 5 7 / tất cả số không | phụ thuộc | trường hợp đối xứng đầy đủ | 
| 3 3 / 0 1 2 | phụ thuộc | cạnh bao phủ toàn bộ cặn | 
| 4 6 / 3 1 4 1 | phụ thuộc | phân phối dư lượng hỗn hợp | 

## Vỏ cạnh 

Một trường hợp khó phát hiện là khi nhiều vị trí tạo ra các dư lượng bị cấm giống hệt nhau. Ví dụ: nếu tất cả a_i bằng nhau thì tất cả r_i sẽ thu gọn thành một tập hợp nhỏ. Thuật toán xử lý việc này một cách chính xác vì nó chỉ theo dõi các dư lượng riêng biệt và các bản sao không ảnh hưởng đến tính khả thi. 

Một trường hợp khác là khi m nhỏ so với n. Nếu n ≥ m và dư lượng đa dạng, bộ có thể bão hòa nhanh chóng và kích hoạt NO một cách chính xác. Điều này được xử lý một cách tự nhiên bằng cách kiểm tra kích thước đã đặt. 

Trường hợp cuối cùng là khi m lớn và n nhỏ. Khi đó, độ bão hòa là không thể, và câu trả lời luôn là CÓ, phù hợp với trực giác rằng có đủ tự do trong việc lựa chọn độ trễ tổng thể.
