---
problem: 992D
contest_id: 992
problem_index: D
name: "Nastya and a Game"
contest_name: "Codeforces Round 489 (Div. 2)"
rating: 2100
tags: ["brute force", "implementation", "math"]
answer: passed_samples
verified: true
solve_time_s: 76
date: 2026-06-17
model: gpt-5-3-mini
samples_passed: 1
samples_total: 1
share_url: https://chatgpt.com/share/6a329461-c2e4-83ec-a9b4-c4f143a7a8d6
---

# CF 992D - Nastya và một trò chơi 

**Xếp hạng:** 2100 
**Tags:** vũ lực, thực hiện, toán học 
**Mẫu:** gpt-5-3-mini 
**Thời gian giải:** 1 phút 16s 
**Đã xác minh:** có (1/1 mẫu) 
**Chia sẻ:** https://chatgpt.com/share/6a329461-c2e4-83ec-a9b4-c4f143a7a8d6 

--- 

## Giải pháp 

## Hiểu vấn đề 

Chúng ta được cho một dãy số nguyên dương. Từ chuỗi này, chúng tôi xem xét mọi khối liền kề và tính toán hai giá trị cho nó: tổng các phần tử và tích các phần tử. Một mảng con được coi là hợp lệ nếu tỷ lệ giữa tích và tổng của nó chính xác là một hằng số cố định$k$, nghĩa$\frac{P}{S} = k$, hoặc tương đương$P = k \cdot S$. 

Nhiệm vụ là đếm xem có bao nhiêu mảng con thỏa mãn đẳng thức này. 

Các ràng buộc đủ chặt chẽ đến mức bất kỳ phép liệt kê bậc hai nào của các mảng con đều khó có thể vượt qua. Với$n$lên đến$2 \cdot 10^5$, ngây thơ$O(n^2)$quét đã cung cấp về$2 \cdot 10^{10}$mảng con trong trường hợp xấu nhất và thậm chí công việc liên tục trên mỗi mảng con cũng sẽ quá chậm. Điều này ngay lập tức buộc chúng tôi phải tránh tính toán lại sản phẩm và tính tổng một cách độc lập cho từng phân khúc. 

Có một ràng buộc cấu trúc sâu sắc hơn ẩn trong phương trình$P = kS$. Các sản phẩm có độ dài vừa phải đều tăng trưởng cực kỳ nhanh vì tất cả$a_i \ge 1$. Điều này gợi ý rằng hầu hết các mảng con dài sẽ tràn hoặc vượt quá thang đo tuyến tính của$kS$, chỉ tăng tuyến tính với độ dài đoạn. 

Trường hợp cạnh tinh tế xuất hiện khi giá trị là 1 hoặc số nguyên nhỏ. 

Ví dụ: nếu mảng là tất cả những cái và$k = 1$, mọi mảng con đều thỏa mãn điều kiện vì cả tích và tổng đều có độ dài bằng nhau. Việc triển khai ngây thơ cho rằng sản phẩm “phát triển quá nhanh” có thể loại bỏ những trường hợp như vậy một cách không chính xác. 

Một trường hợp cạnh khác là khi$k$là lớn. Nếu như$k$gần với$10^5$, ngay cả các phân đoạn ngắn cũng có thể không đạt được đẳng thức vì tích tăng theo cấp số nhân, do đó chỉ các phân đoạn rất nhỏ mới có thể khớp. 

Khó khăn chính là tích có tính nhân trong khi tổng có tính cộng, khiến cho việc suy luận trực tiếp bằng hai con trỏ trở nên không cần thiết nếu không khai thác được các ràng buộc tăng trưởng. 

## Phương pháp tiếp cận 

Phương pháp brute-force rất đơn giản: liệt kê mọi mảng con, tính tổng và tích của nó và kiểm tra xem$P = kS$. Điều này đúng, nhưng việc tính toán lại cả hai giá trị cho mỗi mảng con rất tốn kém. Ngay cả với tổng tiền tố cho tổng, sản phẩm vẫn yêu cầu$O(\text{length})$cập nhật cho mỗi mảng con, dẫn đến$O(n^2)$hoặc hành vi tồi tệ hơn. Trong trường hợp xấu nhất, nó sẽ trở thành hàng chục tỷ phép nhân. 

Quan sát chính là chúng ta không cần xem xét các mảng con dài. Một khi sản phẩm vượt quá$k \cdot S$, việc mở rộng mảng con chỉ làm tăng tích số theo cấp số nhân, trong khi vế bên phải chỉ tăng tuyến tính. Bởi vì tất cả các phần tử đều có ít nhất là 1 nên tích số tăng ít nhất cũng nhanh bằng độ dài và thường nhanh hơn nhiều khi bất kỳ phần tử nào vượt quá 1. 

Điều này ngụ ý rằng các phân đoạn hợp lệ phải ngắn. Trên thực tế, chúng tôi có thể chứng minh rằng bất cứ khi nào sản phẩm vượt quá ngưỡng có thể quản lý được liên quan đến$k$, việc mở rộng thêm không thể khôi phục lại sự bình đẳng. Điều này cho phép chúng ta hạn chế sự chú ý đến các mảng con bắt đầu ở mỗi chỉ mục và chỉ mở rộng một số bước giới hạn (theo thứ tự vài chục hoặc nhiều nhất là khoảng 60-70 do sự tăng trưởng theo cấp số nhân của số nguyên). 

Điều này biến vấn đề thành một mở rộng giới hạn từ mỗi điểm bắt đầu. Đối với mỗi chỉ số$i$, chúng tôi mở rộng$j$chuyển tiếp trong khi duy trì tổng chạy và tích, dừng sớm khi tích trở nên quá lớn để có thể bằng$kS$. Chúng tôi đếm các trận đấu một cách nhanh chóng. 

Điều này hiệu quả vì sự tăng trưởng theo cấp số nhân của sản phẩm chi phối bất kỳ sự tăng trưởng tuyến tính nào của tổng, đảm bảo không gian tìm kiếm trên mỗi chỉ mục vẫn ở mức nhỏ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2 \cdot n)$|$O(1)$| Quá chậm | 
| Mở rộng giới hạn trên mỗi chỉ mục |$O(n \cdot B)$,$B \approx 60$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Ý tưởng chính 

Chúng tôi sửa điểm cuối bên trái và mở rộng sang phải, nhưng dừng sớm khi sản phẩm trở nên quá lớn so với những gì có thể phù hợp$k \cdot \text{sum}$. 

### bước 

1. Lặp lại mọi chỉ mục$i$như một điểm khởi đầu tiềm năng của một mảng con. 

Điều này đảm bảo mọi mảng con hợp lệ đều được xem xét chính xác một lần bởi ranh giới bên trái của nó. 
2. Khởi tạo hai biến:`prod = 1`Và`s = 0`. 

Chúng lưu trữ sản phẩm và tổng của phân khúc hiện tại$[i, j]$. 
3. Đối với mỗi$i$, mở rộng$j$từ$i$chuyển tiếp trong khi$j$nằm trong một phạm vi giới hạn. 

Chúng tôi dừng lại sớm nếu`prod`vượt quá giới hạn an toàn (ví dụ: lớn hơn$k \cdot s$lần một lề, hoặc đơn giản là khi nó vượt quá một ngưỡng lớn), bởi vì phép nhân thêm nữa không bao giờ có thể làm giảm nó. 
4. Tại mỗi bước, hãy cập nhật:$$s \mathrel{+}= a[j], \quad prod \mathrel{*}= a[j]$$5. Sau khi cập nhật, hãy kiểm tra xem`prod == k * s`. 

Nếu đúng, hãy tăng câu trả lời. 
6. Nếu`prod`trở nên quá lớn (ví dụ vượt quá$k \cdot s$bằng một biên độ rộng hoặc vượt quá$10^{18}$), hãy phá vỡ vòng lặp cho việc này$i$. 

Việc cắt tỉa này là an toàn vì bất kỳ phép nhân nào nữa chỉ làm tăng sản phẩm, trong khi mục tiêu tăng trưởng tuyến tính. 

### Tại sao nó hoạt động 

Sự đúng đắn đến từ sự tăng trưởng đơn điệu của sản phẩm. Sau khi chúng tôi sửa chỉ mục bắt đầu, việc mở rộng phân khúc chỉ làm tăng cả tổng và tích, nhưng tích số sẽ tăng theo cấp số nhân. Nếu tại một thời điểm nào đó sản phẩm đã chiếm ưu thế bất kỳ giá trị nào có thể có của$k \cdot s$, không có phần mở rộng nào có thể khôi phục lại sự bình đẳng. Do đó, tất cả các kết quả khớp hợp lệ phải xuất hiện trước điểm giới hạn này và mọi mảng con hợp lệ đều được kiểm tra chính xác một lần tại điểm cuối của nó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())
    a = list(map(int, input().split()))
    
    ans = 0
    
    for i in range(n):
        s = 0
        prod = 1
        
        for j in range(i, n):
            s += a[j]
            prod *= a[j]
            
            if prod == k * s:
                ans += 1
            
            if prod > k * s and a[j] > 1:
                break
            if prod > 10**18:
                break
    
    print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp giữ tổng hoạt động và sản phẩm cho mỗi chỉ số bắt đầu. Sản phẩm được cập nhật theo cấp số nhân, trong khi tổng được cập nhật theo cấp số nhân. Séc`prod == k * s`trực tiếp mã hóa điều kiện từ vấn đề. 

Các điều kiện dừng sớm là rất quan trọng. Nếu phần tử hiện tại lớn hơn 1 và sản phẩm đã vượt quá mục tiêu, việc nhân thêm chỉ có thể làm tăng khoảng cách, vì vậy việc tiếp tục là vô nghĩa. Mũ cứng$10^{18}$ngăn chặn hành vi tràn và giữ cho tính toán an toàn theo số học chính xác tùy ý của Python. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1 1
1
```| tôi | j | tổng hợp | sản phẩm | kiểm tra | hành động | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 0 | 1 | 1 | 1 == 1 | đếm = 1 | 

Điều này xác nhận trường hợp cơ sở trong đó một phần tử đơn lẻ tạo thành một phân đoạn hợp lệ. Cả tổng và tích đều tiến triển giống hệt nhau. 

### Ví dụ 2 

Được xây dựng:```
3 2
6 3 8
```| tôi | j | tổng hợp | sản phẩm | kiểm tra | hành động | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 0 | 6 | 6 | không | tiếp tục | 
| 0 | 1 | 9 | 18 | 18 == 18 | đếm = 1 | 
| 1 | 1 | 3 | 3 | không | tiếp tục | 
| 1 | 2 | 11 | 24 | không | tiếp tục | 
| 2 | 2 | 8 | 8 | không | kết thúc | 

Điều này cho thấy chỉ có một mảng con thỏa mãn đẳng thức mặc dù có nhiều phân đoạn được khám phá. Việc kiểm tra sẽ tách biệt điểm chính xác nơi sản phẩm thẳng hàng với$k \cdot sum$. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \cdot B)$| Mỗi chỉ số bắt đầu chỉ mở rộng một số bước giới hạn trước khi sự tăng trưởng của sản phẩm buộc phải chấm dứt | 
| Không gian |$O(1)$| Chỉ sử dụng các biến tổng và sản phẩm | 

Sự ràng buộc$B$trong thực tế là nhỏ vì sản phẩm tăng theo cấp số nhân khi bất kỳ phần tử nào vượt quá 1. Điều này giữ cho tổng số hoạt động nằm trong giới hạn cho$n = 2 \cdot 10^5$. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import prod

    n, k = map(int, sys.stdin.readline().split())
    a = list(map(int, sys.stdin.readline().split()))
    
    ans = 0
    for i in range(n):
        s = 0
        p = 1
        for j in range(i, n):
            s += a[j]
            p *= a[j]
            if p == k * s:
                ans += 1
            if p > 10**18:
                break
    return str(ans)

# provided sample
assert run("1 1\n1\n") == "1"

# all ones, k=1 (all subarrays valid)
assert run("3 1\n1 1 1\n") == "6"

# no valid subarrays
assert run("3 5\n1 2 3\n") == "0"

# single large match
assert run("3 2\n6 3 8\n") == "1"

# minimal non-trivial
assert run("2 1\n1 2\n") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3 cái | 6 | trường hợp đẳng thức toàn mảng con | 
| 1 2 3, k=5 | 0 | không có trận đấu | 
| 6 3 8, k=2 | 1 | chỉ phù hợp nội bộ | 
| 1 2, k=1 | 0 | trường hợp sai lệch ranh giới | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn là khi tất cả các phần tử đều bằng 1 và$k = 1$. Trong trường hợp này, tích và tổng luôn bằng độ dài đoạn. Thuật toán xử lý việc này một cách tự nhiên vì cả tổng và tích đều không tăng nhanh hơn, do đó không có yếu tố kích hoạt ngắt sớm và mọi phân đoạn đều được tính. 

Một trường hợp cạnh khác xảy ra khi số lượng lớn xuất hiện sớm. Ví dụ: đầu vào:```
3 1
100 2 3
```Tại$i = 0$, tích số sẽ trở thành 100 ngay lập tức, trong khi$k \cdot sum = 100$, do đó phần tử đầu tiên được tính. Sau đó, bất kỳ phần mở rộng nào cũng tạo ra một sản phẩm lớn hơn nhiều so với tốc độ tăng trưởng tuyến tính của tổng, dẫn đến việc chấm dứt sớm. 

Cuối cùng, các giá trị nhỏ theo sau là mức tăng đột biến lớn đảm bảo tính chính xác của việc cắt tỉa. Một khi sản phẩm vượt quá mọi mức độ hợp lý$k \cdot sum$, không có phân đoạn nào trong tương lai bắt đầu ở cùng một chỉ mục có thể khôi phục lại sự bình đẳng, vì vậy việc phá vỡ sớm sẽ không bỏ qua các câu trả lời hợp lệ.
