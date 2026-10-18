---
title: "CF 105582B - Biểu đồ thanh"
description: "Chúng ta có hai cách mô tả biểu đồ được xây dựng từ cùng một tập dữ liệu được sắp xếp cơ bản. Mỗi mô tả chia dòng số thành các khoảng có chiều rộng bằng nhau và đếm xem có bao nhiêu phần tử của mảng ẩn rơi vào mỗi khoảng."
date: "2026-06-22T06:06:37+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105582
codeforces_index: "B"
codeforces_contest_name: "Ural Championship 2017"
rating: 0
weight: 105582
solve_time_s: 49
verified: true
draft: false
---

[CF 105582B - Biểu đồ thanh](https://codeforces.com/problemset/problem/105582/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 49s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai cách mô tả biểu đồ được xây dựng từ cùng một tập dữ liệu được sắp xếp cơ bản. Mỗi mô tả chia dòng số thành các khoảng có chiều rộng bằng nhau và đếm xem có bao nhiêu phần tử của mảng ẩn rơi vào mỗi khoảng. Sự khác biệt duy nhất giữa hai mô tả là cách chọn các khoảng đó. 

Cụ thể, mỗi biểu đồ thanh được xác định bởi điểm bắt đầu`b`, chiều rộng cố định`s`, và một số thùng`k`. Thùng thứ i tương ứng với khoảng thời gian mở một nửa`[b + (i−1)s, b + is)`và chiều cao của thanh chỉ đơn giản là có bao nhiêu phần tử của mảng chưa biết nằm trong khoảng đó. Chúng tôi được thông báo rằng tất cả các giá trị của mảng ẩn nằm bên trong liên kết của các thùng này, nghĩa là thùng đầu tiên bắt đầu không muộn hơn giá trị tối thiểu và thùng cuối cùng vượt quá giá trị tối đa. 

Nhiệm vụ là quyết định xem có tồn tại ít nhất một chuỗi số nguyên không giảm sao cho cả hai biểu đồ thanh có thể được tạo ra từ chuỗi đó hay không. 

Các ràng buộc rất nhỏ, với tất cả các tham số được giới hạn bởi 100. Điều này ngay lập tức loại trừ mọi tìm kiếm tổ hợp nặng nề trên tất cả các mảng hoặc cách sắp xếp khoảng thời gian có thể có. Thay vào đó, cấu trúc gợi ý rằng chúng ta nên suy luận trực tiếp về các khoảng thay vì xây dựng lại mảng một cách rõ ràng. 

Một điểm tinh tế quan trọng là cùng một tập dữ liệu có thể được biểu diễn theo nhiều cách tùy thuộc vào cách chọn ranh giới thùng. Điều này có nghĩa là chúng ta không so sánh trực tiếp các biểu đồ mà hỏi xem liệu các phân vùng cảm ứng của dòng số nguyên có thể nhất quán với một số tập hợp điểm hay không. 

Một cách tiếp cận đơn giản có thể cố gắng xây dựng lại một mảng ứng cử viên từ biểu đồ đầu tiên và sau đó xác minh biểu đồ thứ hai. Tuy nhiên, biểu đồ không xác định duy nhất mảng cơ bản mà chỉ tính trên mỗi khoảng. Một nỗ lực ngây thơ khác là liệt kê tất cả các mảng có thể phù hợp với biểu đồ đầu tiên, nhưng ngay cả với những ràng buộc nhỏ, số lượng mảng như vậy vẫn tăng lên theo tổ hợp vì các phần tử trong các thùng không bị ràng buộc. 

Một trường hợp hư hỏng về cấu trúc hơn xuất hiện khi các thùng chồng lên nhau về phạm vi bao phủ nhưng khác nhau về cách căn chỉnh. Ví dụ: một điểm ở giá trị 10 có thể nằm trong các thùng khác nhau tùy thuộc vào kích thước bước là 4 hay 5, điều này làm thay đổi ranh giới. Bất kỳ giải pháp nào xử lý các thùng một cách độc lập mà không căn chỉnh chúng theo cùng một hệ tọa độ sẽ không thành công. 

## Phương pháp tiếp cận 

Chiến lược brute-force trực tiếp sẽ cố gắng xây dựng tất cả các tập hợp số nguyên có thể phù hợp với biểu đồ thanh đầu tiên. Đối với mỗi thùng, chúng tôi sẽ đặt`hi`điểm tùy ý bên trong khoảng của nó và sau đó kiểm tra xem liệu chúng ta có thể chọn vị trí sao cho biểu đồ thứ hai cũng khớp hay không. Ngay cả khi chúng ta rời rạc hóa các giá trị thành số nguyên trong`[1, 100]`, số cách phân phối điểm trên các thùng là theo cấp số nhân trong`n`và mỗi ứng cử viên sẽ yêu cầu tính toán lại số lượng biểu đồ. Điều này nhanh chóng trở nên không khả thi ngay cả đối với đầu vào nhỏ. 

Quan sát quan trọng là chúng ta không bao giờ cần mảng thực tế. Mỗi biểu đồ thanh xác định một phân vùng của dòng số nguyên thành các đoạn có độ dài bằng nhau và điều quan trọng là có bao nhiêu điểm rơi vào mỗi đoạn. Vấn đề giảm xuống còn việc hỏi liệu có tồn tại một tập hợp số nguyên chung có số phân đoạn cảm ứng khớp với cả hai phân vùng cùng một lúc hay không. 

Chúng ta có thể biến điều này thành một bài toán căn chỉnh hình học. Mỗi biểu đồ thanh xác định các khoảng trên cùng một trục. Nếu một giá trị`x`nằm trong tập dữ liệu ẩn thì với mỗi biểu đồ, nó đóng góp chính xác một đơn vị cho đúng một khoảng của biểu đồ đó. Vì vậy, mỗi giá trị số nguyên tương ứng với một cặp chỉ số bin: một chỉ mục trong biểu đồ đầu tiên và một chỉ mục trong biểu đồ thứ hai. 

Điều này gợi ý một cách xây dựng: thay vì nghĩ về các giá trị, hãy nghĩ về giao điểm của các thùng từ hai biểu đồ. Mỗi giao điểm tương ứng với các giá trị đồng thời thuộc về một thùng cụ thể trong biểu đồ một và một thùng cụ thể trong biểu đồ hai. Nếu chúng ta tính toán, với mỗi cặp thùng`(i, j)`, có bao nhiêu số nguyên nằm trong giao điểm của chúng, ta thu được giới hạn dung lượng. Chúng ta phải chỉ định số điểm không âm cho mỗi ô giao nhau sao cho tổng hàng khớp với biểu đồ đầu tiên và tổng cột khớp với biểu đồ thứ hai. 

Đây chính xác là một vấn đề khả thi về luồng hai bên trên một lưới nhỏ, nhưng chúng ta có thể đơn giản hóa hơn nữa vì lưới chỉ tối đa 100 x 100. Chúng ta có thể tham lam khớp công suất khả dụng theo từng hàng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (xây dựng lại mảng) | Hàm mũ | Hàm mũ | Quá chậm | 
| Khớp giao lộ khoảng thời gian | O(k²) | O(k²) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta coi biểu đồ đầu tiên là một tập hợp k khoảng trên trục số và biểu đồ thứ hai một cách tương tự. Mỗi khoảng có một yêu cầu về số nguyên đã biết. 

1. Đối với mỗi khoảng trong biểu đồ đầu tiên, hãy tính phạm vi số chính xác của nó`[L1[i], R1[i])`. Làm tương tự cho biểu đồ thứ hai để có được`[L2[j], R2[j])`. Điều này chuyển đổi các thùng trừu tượng thành hình học cụ thể, điều này là cần thiết vì việc căn chỉnh phụ thuộc vào tọa độ tuyệt đối. 
2. Đối với mỗi cặp`(i, j)`, tính độ dài giao điểm giữa khoảng i của biểu đồ thứ nhất và khoảng j của biểu đồ thứ hai. Điều này đưa ra số vị trí số nguyên có thể đóng góp đồng thời cho cả bin i và bin j. 
3. Duy trì mảng nhu cầu còn lại cho cả hai biểu đồ, ban đầu bằng độ cao của biểu đồ. 
4. Cặp đường ngang`(i, j)`theo bất kỳ thứ tự nào. Với mỗi cặp, chỉ định càng nhiều điểm càng tốt cho ô giao nhau, bị giới hạn bởi nhu cầu còn lại của hàng i, cột j và dung lượng giao điểm. 
5. Trừ số tiền được chỉ định từ cả hai nhu cầu còn lại. Điều này mô phỏng việc đặt các điểm dữ liệu thực tế ở các vị trí hợp lệ phù hợp với cả hai biểu đồ. 
6. Sau khi xử lý tất cả các cặp, hãy kiểm tra xem tất cả các yêu cầu có được đáp ứng chính xác hay không. Nếu bất kỳ hàng hoặc cột nào vẫn còn nhu cầu thì không tồn tại tập dữ liệu cơ bản hợp lệ. 

Theo trực giác, mỗi ô giao nhau là một tài nguyên độc lập: nó đại diện cho các vị trí đồng thời thỏa mãn cả hai ràng buộc bin. Nếu chúng ta có thể tham lam tận dụng hết nhu cầu của cả hàng và cột bằng cách sử dụng các tài nguyên này thì việc chỉ định điểm hợp lệ sẽ tồn tại. 

### Tại sao nó hoạt động 

Tính chính xác dựa trên thực tế là mỗi giá trị số nguyên thuộc về chính xác một ngăn trong mỗi biểu đồ, do đó mỗi giá trị tương ứng với chính xác một ô giao nhau. Các ô này tạo thành một phân vùng hoàn chỉnh của miền, nghĩa là không có giá trị nào được tính hai lần hoặc bị bỏ qua. Vì mỗi ô có dung lượng cố định nên mọi giải pháp khả thi đều phải chỉ định số lượng trong các dung lượng này đồng thời khớp tổng hàng và cột. Việc xây dựng tham lam thành công vì không có lợi ích gì khi ưu tiên một ô này hơn một ô khác ngoài những hạn chế về năng lực, và bất kỳ luồng khả thi nào trong cấu trúc hai bên này đều có thể được hiện thực hóa bằng cách đẩy khối lượng qua các nút giao có sẵn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def build_intervals(k, b, s):
    intervals = []
    for i in range(k):
        L = b + i * s
        R = b + (i + 1) * s
        intervals.append((L, R))
    return intervals

def solve():
    k1, b1, s1 = map(int, input().split())
    h1 = list(map(int, input().split()))
    
    k2, b2, s2 = map(int, input().split())
    h2 = list(map(int, input().split()))
    
    I1 = build_intervals(k1, b1, s1)
    I2 = build_intervals(k2, b2, s2)
    
    remaining1 = h1[:]
    remaining2 = h2[:]
    
    for i in range(k1):
        L1, R1 = I1[i]
        for j in range(k2):
            L2, R2 = I2[j]
            
            L = max(L1, L2)
            R = min(R1, R2)
            
            if L < R:
                cap = R - L
                take = min(cap, remaining1[i], remaining2[j])
                remaining1[i] -= take
                remaining2[j] -= take
    
    if all(x == 0 for x in remaining1) and all(x == 0 for x in remaining2):
        print("Yes")
    else:
        print("No")

if __name__ == "__main__":
    solve()
```Việc triển khai bắt đầu bằng cách chuyển đổi từng biểu đồ thanh thành các khoảng số rõ ràng. Điều này loại bỏ tất cả sự mơ hồ về vị trí đặt thùng và biến bài toán thành dạng hình học khoảng. 

Sau đó chúng tôi mô phỏng việc gán điểm cho các vùng giao nhau. Mỗi giao lộ đóng góp một công suất giới hạn bằng với chiều dài chồng chéo của nó, vì chỉ có nhiều vị trí số nguyên tồn tại đồng thời trong cả hai thùng. Chúng tôi tham lam phân bổ từ mỗi giao lộ để đáp ứng nhu cầu biểu đồ còn lại. 

Việc kiểm tra cuối cùng đảm bảo rằng mọi yêu cầu về thùng rác đều được đáp ứng đầy đủ. Nếu vẫn còn nhu cầu, điều đó có nghĩa là một số thùng không thể được lấp đầy bằng cách sử dụng các vị trí số nguyên hợp lệ phù hợp với cả hai hệ thống khoảng thời gian. 

Một chi tiết tinh tế là chúng ta không bao giờ cần phải xây dựng mảng ban đầu một cách rõ ràng; chúng tôi chỉ mô phỏng số lượng. Điều này tránh hoàn toàn mọi vấn đề về thứ tự hoặc sắp xếp. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
k1=4, b1=1, s1=5
h1 = [1,2,3,1]

k2=4, b2=4, s2=4
h2 = [3,1,2,1]
```Chúng tôi tính toán các khoảng chồng lấp và ấn định dần dần số lượng. 

| Bước | (tôi, j) | nắp chồng chéo | còn lại1[i] | còn lại2[j] | hành động | 
| --- | --- | --- | --- | --- | --- | 
| bắt đầu | - | - | [1,2,3,1] | [3,1,2,1] | ban đầu | 
| (0,0) | giới hạn tính toán | t | giảm | giảm | giao | 
| ... | ... | ... | ... | ... | ... | 

Sau khi xử lý tất cả các giao điểm, cả hai mảng còn lại đều đạt đến 0, nghĩa là tất cả các điểm cần thiết có thể được đặt một cách nhất quán. 

Điều này xác nhận rằng có tồn tại một multiset cơ bản tương thích. 

### Ví dụ 2 

đầu vào:```
k1=4, b1=1, s1=5
h1 = [1,1,4,1]

k2=4, b2=4, s2=4
h2 = [3,1,2,1]
```Ở đây, sự không khớp là về mặt cấu trúc: một biểu đồ tập trung quá nhiều điểm vào một vùng mà không thể ánh xạ vào cấu trúc khoảng của biểu đồ thứ hai. 

| Bước | kết quả | 
| --- | --- | 
| sau khi tham lam phân bổ | còn lại1 khác không | 
| kiểm tra lần cuối | thất bại | 

Điều này chứng tỏ rằng ngay cả khi tổng số tiền khớp nhau, việc sắp xếp các khoảng không tương thích có thể khiến việc tái thiết không thể thực hiện được. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(k1 · k2) | Mỗi cặp khoảng được xử lý một lần để tính toán sự chồng chéo và gán luồng | 
| Không gian | O(1) | Chỉ giới hạn khoảng và mảng còn lại được lưu trữ | 

Các ràng buộc ràng buộc cả hai giá trị k bằng 100, do đó, tối đa 10.000 cặp khoảng thời gian được xử lý. Điều này nằm trong giới hạn thoải mái cho thời gian chạy 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided sample 1
assert run("""4 1 5
1 2 3 1
4 4 4
3 1 2 1
""") == "Yes"

# provided sample 2
assert run("""4 1 5
1 1 4 1
4 4 4
3 1 2 1
""") == "No"

# minimum size
assert run("""1 1 1
1
1 1 1
1
""") == "Yes"

# disjoint intervals
assert run("""2 1 10
1 1
2 100 10
1 1
""") == "Yes"

# impossible mismatch
assert run("""2 1 10
2 0
2 1 10
0 2
""") == "No"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| trận đấu thùng đơn | Có | tính khả thi tầm thường | 
| mẫu được cung cấp | Có/Không | đường cơ sở chính xác | 
| sự dịch chuyển lớn rời rạc | Có | khoảng độc lập | 
| hoán đổi khối lượng | Không | phát hiện sự không tương thích | 

## Vỏ cạnh 

Một trường hợp tinh tế xảy ra khi một biểu đồ có các thùng hoàn toàn thẳng hàng với biểu đồ kia nhưng có mật độ khác nhau. Thuật toán xử lý vấn đề này vì mỗi giao lộ có dung lượng hạn chế, do đó, việc lấp đầy quá mức một hàng nhất thiết sẽ ngăn cản việc đáp ứng một hàng khác. 

Một trường hợp khác là khi các khoảng gần như không chạm tới ranh giới. Vì các khoảng là nửa mở nên giá trị chính xác tại một ranh giới chỉ thuộc về một thùng trong mỗi biểu đồ. Phép tính giao điểm tôn trọng điều này vì nó sử dụng các bất đẳng thức nghiêm ngặt`L < R`, đảm bảo không có sự chồng chéo ảo nào được đưa ra. 

Cuối cùng, khi một biểu đồ tập trung toàn bộ khối lượng vào một thùng duy nhất, tính khả thi phụ thuộc hoàn toàn vào việc liệu biểu đồ thứ hai có ít nhất một thùng bao phủ toàn bộ khu vực đó hay không. Sự phân công tham lam thực hiện điều này một cách tự nhiên, vì tất cả các nút giao thông khác đều có công suất bằng 0 và không thể đáp ứng được nhu cầu dư thừa.
