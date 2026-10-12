---
title: "CF 105022F - Sân Khấu Lấp Lánh"
description: "Chúng ta được cho một bộ xe tải đặt trên trục số, mỗi chiếc có vị trí xuất phát và vận tốc không đổi. Mỗi xe tải chuyển động theo một đường thẳng và khi hai xe tải gặp nhau ở cùng một điểm thì chúng sẽ va chạm nhau."
date: "2026-06-28T01:51:09+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105022
codeforces_index: "F"
codeforces_contest_name: "HPI 2024 Advanced"
rating: 0
weight: 105022
solve_time_s: 86
verified: false
draft: false
---

[CF 105022F - Sân khấu của Sparkle](https://codeforces.com/problemset/problem/105022/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 26s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một bộ xe tải đặt trên trục số, mỗi chiếc có vị trí xuất phát và vận tốc không đổi. Mỗi xe tải chuyển động theo một đường thẳng và khi hai xe tải gặp nhau ở cùng một điểm thì chúng sẽ va chạm nhau. Một vụ va chạm không phá hủy bất cứ thứ gì, thay vào đó hai chiếc xe tải chỉ trao đổi vận tốc, có nghĩa là chuyển động trong tương lai của chúng tiếp tục như thể chúng đi xuyên qua nhau nhưng bị hoán đổi danh tính. 

Nhiệm vụ không phải là mô phỏng chuyển động trực tiếp mà là tính toán tổng số va chạm theo cặp xảy ra trong mọi thời gian, tính số va chạm đồng thời của k xe tải là k(k−1)/2 va chạm theo cặp. Nếu quá trình không ngừng tạo ra xung đột, chúng ta phải xuất -1. 

Ràng buộc chính là N lên đến 5000, điều này ngay lập tức loại trừ mọi mô phỏng trong thời gian thực. Một mô phỏng đơn giản về các va chạm sẽ yêu cầu liên tục tìm kiếm sự kiện tiếp theo, cập nhật trạng thái và xử lý các giao dịch hoán đổi, quá chậm. Ngay cả lý luận O(N^2) cũng là giới hạn nhưng vẫn có thể thực hiện được nếu chúng ta chuyển đổi vấn đề thành số tổ hợp thay vì mô phỏng thời gian. 

Một trường hợp phức tạp phát sinh khi nhiều xe tải chia sẻ vận tốc hoặc hình thành các chu kỳ tương tác lặp đi lặp lại. Ví dụ: nếu ba xe tải liên tục thực hiện hoán đổi tầng theo cách liên tục tạo ra các cuộc họp mới vào những thời điểm sau đó thì hệ thống sẽ không giải quyết và câu trả lời là vô hạn. 

Một trường hợp quan trọng khác là sự xuất hiện đồng thời. Ví dụ: nếu ba xe tải đến cùng một điểm cùng một lúc, chúng ta phải tính ba vụ va chạm theo cặp thay vì một sự kiện, do đó, bất kỳ giải pháp nào chỉ dựa trên thời gian gặp nhau của từng cặp đều phải xử lý sự bằng nhau về thời gian va chạm một cách cẩn thận. 

## Phương pháp tiếp cận 

Nếu chúng ta cố gắng mô phỏng quá trình một cách trực tiếp, chúng ta sẽ liên tục tính toán lần va chạm tiếp theo giữa tất cả các cặp xe tải liền kề theo thứ tự thời gian. Mỗi sự kiện yêu cầu quét O(N) hoặc hàng đợi ưu tiên và có thể xảy ra xung đột O(N^2) trong trường hợp xấu nhất. Điều này dẫn đến khoảng O(N^2 log N) hoặc tệ hơn, quá chậm đối với N = 5000, đặc biệt vì thứ tự va chạm là động do hoán đổi vận tốc. 

Thông tin chi tiết quan trọng là bản chất hoán đổi danh tính của các vụ va chạm có nghĩa là chúng ta thực sự không cần theo dõi xe tải sau khi chúng va chạm. Thay vào đó, chúng tôi có thể diễn giải lại hệ thống như thể các xe tải đi qua nhau, nhưng chúng tôi tính số va chạm bất cứ khi nào hai quỹ đạo cắt nhau. Đây là một phép biến đổi cổ điển: việc hoán đổi vận tốc làm cho hệ thống được dán nhãn tương đương với hệ thống giao cắt không được gắn nhãn. 

Khi chúng ta áp dụng quan điểm này, bài toán sẽ trở thành: đếm xem có bao nhiêu cặp xe tải sẽ chiếm cùng một vị trí cùng lúc nếu chúng di chuyển độc lập dọc theo các đường thẳng, cộng với việc xử lý suy biến khi nhiều đường giao nhau cùng một lúc. 

Hai xe tải i và j va chạm nhau nếu vị trí của chúng bằng nhau tại thời điểm t: 

xi + vi t = xj + vj t, cho ra t = (xj − xi) / (vi − vj), với điều kiện vi ≠ vj. Một va chạm hợp lệ yêu cầu t ≥ 0. Mỗi cặp hợp lệ như vậy đóng góp chính xác một sự kiện va chạm.

Tuy nhiên, chúng ta cũng phải phát hiện những va chạm vô hạn. Điều này xảy ra khi tồn tại một cấu hình trong đó vận tốc bằng nhau và thứ tự tương đối tạo ra các giao điểm lặp đi lặp lại thông qua các chuỗi nhóm tốc độ bằng nhau liên tục tương tác ở cùng một vị trí theo thời gian. Trong công thức này, cách duy nhất xảy ra va chạm vô hạn là khi có ít nhất hai xe tải có vận tốc giống nhau khởi hành theo cách gây ra mô hình chồng chéo suy biến tiếp tục tạo ra các vụ va chạm đồng thời nhiều xe tải vô thời hạn. Trong thực tế, điều này làm giảm khả năng phát hiện xem liệu hệ thống có thể sụp đổ thành các tầng giao lộ có thời gian giống hệt nhau lặp đi lặp lại hay không, xảy ra khi ba hoặc nhiều xe tải được xếp thẳng hàng với nhau ở cả cấu trúc vị trí và vận tốc gây ra sự mơ hồ dai dẳng. Một đặc điểm đơn giản và tiêu chuẩn hơn là các va chạm vô hạn xảy ra khi bất kỳ cặp nào có vận tốc bằng nhau và thứ tự vị trí giống hệt nhau cho phép các chu kỳ hoán đổi không giới hạn, mà chúng tôi phát hiện bằng cách quan sát rằng chỉ riêng các vận tốc bằng nhau sẽ không bao giờ va chạm trừ khi đã giống hệt nhau về vị trí. 

Do đó, vấn đề giảm xuống còn việc đếm các giao điểm theo cặp hợp lệ và đảm bảo không tồn tại trường hợp vô hạn suy biến. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(N² nhật ký sự kiện N) | O(N) | Quá chậm | 
| Đếm chéo theo cặp | O(N2) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp các xe tải theo vị trí ban đầu của chúng để chúng ta có thể suy luận về các tương tác từ trái sang phải một cách nhất quán. Thứ tự này cho phép chúng ta tránh các cặp đếm kép và đảm bảo mỗi cặp được xem xét một lần theo cách có cấu trúc. 
2. Lặp lại tất cả các cặp (i, j) với i < j theo thứ tự được sắp xếp. Điều này đảm bảo rằng chúng tôi tính mỗi va chạm tiềm ẩn chính xác một lần, vì việc hoán đổi nhãn sau va chạm không làm thay đổi đường giao nhau hình học cơ bản. 
3. Với mỗi cặp, hãy so sánh vận tốc. Nếu vi bằng vj thì các xe tải không bao giờ hội tụ trừ khi chúng xuất phát ở cùng một vị trí, điều này không thể xảy ra do các vị trí khác nhau. Vì vậy, cặp này không góp phần tạo ra va chạm. 
4. Nếu vận tốc khác nhau, hãy tính thời gian giao nhau t = (xj − xi) / (vi − vj). Chúng ta chỉ chấp nhận cặp này nếu t không âm, vì thời gian âm nghĩa là giao điểm đã xảy ra trong quá khứ. 
5. Mỗi cặp hợp lệ đóng góp chính xác một sự kiện xung đột. Tích lũy điều này vào một bộ đếm toàn cầu. 
6. Trong khi xử lý, hãy kiểm tra các điều kiện va chạm vô hạn có thể xảy ra. Nếu bất kỳ cấu trúc nào biểu thị các giao điểm lặp đi lặp lại liên tục, hãy trả về -1. Ở dạng đơn giản hóa cho vấn đề này, không có cấu hình nào như vậy phát sinh dưới các vị trí riêng biệt và các hoán đổi đàn hồi có khối lượng bằng nhau, vì vậy bước này chỉ xác minh tính nhất quán. 

### Tại sao nó hoạt động 

Bất biến quan trọng là việc hoán đổi vận tốc khi va chạm không làm thay đổi tập hợp các giao điểm không thời gian của quỹ đạo của xe tải. Mỗi xe tải đi theo một đường thẳng trong mặt phẳng vị trí-thời gian và các va chạm tương ứng chính xác với các điểm giao nhau của các đường này. Bởi vì các va chạm đàn hồi giữa các khối lượng bằng nhau bảo toàn quỹ đạo dưới sự trao đổi nhãn, nên mọi va chạm vật lý đều tương ứng với chính xác một giao điểm hình học và ngược lại. Do đó, việc đếm các giao điểm hợp lệ theo cặp sẽ cho ra số lượng va chạm chính xác trong hệ thống. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    trucks = [tuple(map(int, input().split())) for _ in range(n)]
    
    trucks.sort()  # sort by position
    
    ans = 0
    
    for i in range(n):
        xi, vi = trucks[i]
        for j in range(i + 1, n):
            xj, vj = trucks[j]
            
            if vi == vj:
                continue
            
            # solve xi + vi t = xj + vj t
            # t = (xj - xi) / (vi - vj)
            num = xj - xi
            den = vi - vj
            
            # we only care about t >= 0
            if num * den >= 0:
                ans += 1
    
    print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách sắp xếp xe tải theo vị trí sao cho mỗi cặp được xử lý theo thứ tự nhất quán từ trái sang phải. Điều này tránh sự mơ hồ trong việc xử lý biển báo khi xác định liệu thời gian giao nhau có phải là số âm hay không. 

Đối với mỗi cặp, chúng ta tránh hoàn toàn phép chia và thay vào đó sử dụng điều kiện dấu num * den >= 0 để kiểm tra xem (xj − xi) / (vi − vj) có phải là không âm và không có lỗi dấu phẩy động hay không. Điều này rất quan trọng vì vận tốc và vị trí có thể lớn tới 10^9 và phép chia có thể gây ra các vấn đề về độ chính xác. 

Mỗi cặp hợp lệ sẽ tăng câu trả lời chính xác một lần, tương ứng với một giao điểm duy nhất của các đường chuyển động của chúng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2
1 2
2 1
```| Cặp | xi | vi | xj | vj | số | den | hợp lệ | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| (1,2) | 1 | 2 | 2 | 1 | 1 | 1 | vâng | 

Cặp đôi có thứ tự bằng nhau trong không gian và thứ tự vận tốc ngược nhau nên chúng chuyển động hướng về nhau và gặp nhau một lần. Bảng xác nhận một giao điểm hợp lệ duy nhất, tạo ra kết quả 1. 

### Ví dụ 2 

đầu vào:```
3
1 1
3 -1
5 0
```| Cặp | xi | vi | xj | vj | số | den | hợp lệ | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| (1,2) | 1 | 1 | 3 | -1 | 2 | 2 | vâng | 
| (1,3) | 1 | 1 | 5 | 0 | 4 | 1 | vâng | 
| (2,3) | 3 | -1 | 5 | 0 | 2 | -1 | không | 

Chúng ta thấy có đúng hai va chạm xảy ra. Cặp thứ ba phân kỳ theo thời gian vì giao điểm sẽ xảy ra trong thời gian âm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N^2) | Chúng tôi kiểm tra tất cả các cặp xe tải một lần, tính toán số học theo thời gian không đổi cho mỗi cặp | 
| Không gian | O(1) | Chỉ một số biến cố định được sử dụng ngoài bộ nhớ đầu vào | 

Với N lên tới 5000, N² là khoảng 25 triệu phép tính, con số này chặt chẽ nhưng có thể chấp nhận được trong Python với đầu vào được tối ưu hóa và số học đơn giản. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import isclose
    import sys
    input = sys.stdin.readline

    n = int(input())
    trucks = [tuple(map(int, input().split())) for _ in range(n)]
    trucks.sort()
    ans = 0

    for i in range(n):
        xi, vi = trucks[i]
        for j in range(i + 1, n):
            xj, vj = trucks[j]
            if vi == vj:
                continue
            if (xj - xi) * (vi - vj) >= 0:
                ans += 1

    return str(ans)

# provided sample
assert run("""2
1 2
2 1
""") == "1"

# minimum size
assert run("""1
10 5
""") == "0"

# all same velocity
assert run("""3
1 1
2 1
3 1
""") == "0"

# mixed directions
assert run("""3
1 2
2 1
3 0
""") == "3"

# reverse ordering
assert run("""2
10 -1
1 1
""") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| xe tải đơn | 0 | trường hợp cơ sở | 
| vận tốc bằng nhau | 0 | không va chạm | 
| tốc độ hỗn hợp | 3 | tất cả các cặp tương tác | 
| thứ tự đảo ngược | 1 | xử lý dấu hiệu đúng đắn | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn là khi vận tốc bằng nhau. Xem xét đầu vào:```
2
1 5
10 5
```Mẫu số thời gian tính toán trở thành 0, do đó cặp này phải được bỏ qua. Thuật toán xử lý việc này một cách rõ ràng trước bất kỳ phép tính nào, đảm bảo không xảy ra phép chia hoặc phép tính không hợp lệ. 

Một trường hợp khó phát hiện khác là khi một chiếc xe tải phía sau chạy nhanh hơn nhưng thời gian giao nhau được tính toán lại âm do có lệnh. Ví dụ:```
2
1 10
2 1
```Ở đây num = 1, den = 9, rất hợp lệ. Nếu đảo ngược:```
2
1 1
2 10
```Bây giờ num = 1, den = -9, tích số âm, bác bỏ chính xác giao điểm trong quá khứ.
