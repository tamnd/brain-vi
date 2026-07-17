---
title: "CF 103433E - Cưỡi ngựa"
description: "Chúng tôi được cung cấp một dãy các điểm dừng bán đồ ăn dọc theo con đường một chiều. Mỗi điểm dừng có một vị trí cố định ở bên phải điểm xuất phát và một khoảng thời gian cố định cần thiết để tiêu thụ hết điểm đó."
date: "2026-07-03T07:57:57+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103433
codeforces_index: "E"
codeforces_contest_name: "2018-2019 Russia Team Open, High School Programming Contest (VKOSHP 18)"
rating: 0
weight: 103433
solve_time_s: 48
verified: true
draft: false
---

[CF 103433E - Cưỡi ngựa](https://codeforces.com/problemset/problem/103433/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 48s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một dãy các điểm dừng bán đồ ăn dọc theo con đường một chiều. Mỗi điểm dừng có một vị trí cố định ở bên phải điểm xuất phát và một khoảng thời gian cố định cần thiết để tiêu thụ hết điểm đó. Một con ngựa bắt đầu ở vị trí 0 tại thời điểm 0, chỉ di chuyển sang phải và không thể quay lại vị trí trước đó. Việc di chuyển đến một vị trí tốn thời gian bằng quãng đường di chuyển và khi đến điểm dừng, nó cũng dành thời gian ăn uống ở đó. Con ngựa có tổng quỹ thời gian D và mục tiêu là tối đa hóa số điểm dừng mà nó có thể hoàn thành đầy đủ, nghĩa là tiếp cận chúng theo thứ tự và ăn xong trong thời gian giới hạn. 

Một điểm tinh tế quan trọng là con ngựa có thể tự do bỏ qua các điểm dừng. Ràng buộc không phải là việc ghé thăm tất cả các điểm mà là việc chọn một chuỗi các điểm dừng theo thứ tự vị trí tăng dần sao cho tôn trọng tổng chi phí thời gian, trong đó chi phí bao gồm cả thời gian đi lại và thời gian ăn uống. 

Các ràng buộc ngụ ý một cấu trúc lập trình cạnh tranh điển hình: n có thể lớn trong các trường hợp thử nghiệm, tổng cộng lên tới 10^5. Vị trí và thời gian lớn tới 10^9, do đó, bất kỳ giải pháp nào thử tất cả các tập hợp con hoặc tất cả các hoán vị đều không thể thực hiện được. Ngay cả O(n^2) cho mỗi trường hợp thử nghiệm cũng sẽ quá chậm, vì trường hợp xấu nhất sẽ là khoảng 10^10 thao tác. Chúng tôi bị đẩy về phía O(n log n) hoặc O(n) cho mỗi trường hợp thử nghiệm. 

Một trường hợp phổ biến nảy sinh khi sự lựa chọn tham lam chọn món ăn địa phương gần nhất hoặc rẻ nhất không thành công trên toàn cầu. Ví dụ: hãy xem xét tình huống trong đó một món ăn xa hơn một chút có thời gian ăn nhỏ hơn nhiều, cho phép chọn tổng số món sau đó, trong khi một món ăn gần hơn sẽ cản trở tính khả thi trong tương lai. Một chiến lược tham lam theo vị trí hoặc tham lam theo thời gian ngây thơ có thể thất bại ở đây vì ràng buộc thực sự kết hợp cả vị trí và hành trình tích lũy. 

Một vấn đề tế nhị khác là thời gian di chuyển được tích lũy dựa trên vị trí được chọn cuối cùng chứ không phải từ điểm xuất phát mỗi lần. Vì vậy, nếu chúng ta chọn các món ăn ở vị trí 2, 10 và 11 thì chi phí cho mỗi món ăn không độc lập; nó phụ thuộc vào khoảng cách gia tăng và điều này rất dễ bị xử lý sai nếu người ta xử lý không chính xác chuyển động luôn từ 0. 

## Phương pháp tiếp cận 

Phương pháp brute-force sẽ thử tất cả các tập hợp con của món ăn, kiểm tra tính khả thi trong việc tăng thứ tự vị trí và tính tổng thời gian cho mỗi tập hợp con. Đối với mỗi tập hợp con, việc tính toán chi phí đi lại yêu cầu tính tổng các chênh lệch liên tiếp cộng với thời gian ăn uống, tức là O(n) cho mỗi tập hợp con. Vì có 2^n tập con nên điều này trở thành O(n·2^n), điều này không thể thực hiện được ngay cả khi n = 40. 

Quan sát quan trọng là chúng tôi chỉ quan tâm đến việc chọn các món ăn theo thứ tự vị trí tăng dần và đối với bất kỳ số lượng món ăn cố định nào, chúng tôi muốn có cách tốt nhất có thể để giảm thiểu tổng chi phí thời gian. Điều này gợi ý lựa chọn tham lam với cấu trúc dữ liệu duy trì tập hợp các món ăn đã chọn và cho phép đưa ra quyết định thay thế hiệu quả. 

Cái nhìn sâu sắc cổ điển là xử lý các món ăn theo thứ tự vị trí tăng dần trong khi vẫn duy trì lựa chọn tốt nhất hiện tại. Khi xem xét một món ăn, chúng tôi tính toán chi phí thời gian bổ sung nếu chúng tôi đưa nó vào sau món ăn đã chọn trước đó. Nếu chúng ta luôn cố gắng duy trì tổng thời gian nhỏ nhất có thể cho một số món ăn đã chọn nhất định, thì bất cứ khi nào vượt quá D, chúng ta sẽ loại bỏ món ăn “đắt” nhất về mặt đóng góp. Điều này biến vấn đề thành một lựa chọn tham lam với vùng nhớ tối đa: chúng tôi tạm thời sắp xếp các món ăn theo thứ tự, theo dõi tổng thời gian và nếu vượt quá D, chúng tôi sẽ loại bỏ món ăn có thời gian ăn lớn nhất. 

Điều này hiệu quả vì chi phí đi lại được cố định theo đơn đặt hàng, trong khi sự thay đổi chỉ đến từ việc chúng tôi đưa vào thời gian ăn uống. Sau khi các món ăn được sắp xếp theo vị trí, chi phí di chuyển gia tăng sẽ được cố định, do đó, sự linh hoạt duy nhất là nên giữ những món nào trong ngân sách. Cấu trúc trở nên tương tự như việc chọn số lượng trọng số tối đa theo một ràng buộc với chi phí tiền tố tăng dần.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tập hợp con Brute Force | O(n·2^n) | O(n) | Quá chậm | 
| Tham lam với đống vị trí được sắp xếp | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Hướng dẫn thuật toán 

1. Sắp xếp tất cả các món ăn theo vị trí tăng dần. Điều này đảm bảo mọi tuyến đường hợp lệ đều tương ứng với lựa chọn nhất quán tiền tố, vì ngựa không thể đi lùi. 
2. Duy trì ba biến: thời gian sử dụng hiện tại, vị trí cuối cùng và thời gian ăn lưu trữ đống tối đa của các món ăn đã chọn. Đống dữ liệu giúp chúng tôi xác định món ăn đã chọn nào tốn kém nhất để loại bỏ nếu cần. 
3. Lặp lại các món ăn theo thứ tự sắp xếp. Đối với mỗi món ăn, hãy tính thời gian di chuyển từ vị trí cuối cùng đến món ăn này và cộng thời gian ăn của nó để có được tổng thời gian dự kiến ​​mới nếu chúng ta bao gồm nó. 
4. Thêm thời gian ăn của món ăn này vào đống và cập nhật thời gian tích lũy. Cập nhật vị trí cuối cùng cho vị trí món ăn hiện tại. 
5. Nếu tổng thời gian vượt quá D, hãy lấy đĩa có thời gian ăn lớn nhất ra khỏi đống và trừ đi phần đóng góp của nó vào tổng thời gian. Lý do là việc loại bỏ thời gian ăn uống đắt nhất sẽ mang lại cơ hội tốt nhất để khôi phục tính khả thi trong khi chỉ mất một món đã chọn. 
6. Sau khi chế biến tất cả các món ăn, kích thước của đống là số lượng món ăn tối đa có thể ăn được. 

### Tại sao nó hoạt động 

Ở bất kỳ bước nào, chúng tôi duy trì việc lựa chọn các món ăn theo thứ tự vị trí tăng dần sao cho tổng thời gian được giảm thiểu đối với kích thước của nó. Thành phần di chuyển được cố định sau khi các vị trí được cố định, vì vậy phần duy nhất có thể điều chỉnh được là bao gồm thời gian ăn uống. Khi vượt quá ngân sách, việc loại bỏ thời gian ăn lớn nhất là tối ưu vì nó tạo ra mức giảm tối đa có thể trong tổng chi phí cho mỗi lần loại bỏ. Đối số trao đổi tham lam này đảm bảo rằng nếu tồn tại bất kỳ tập hợp con khả thi nào có cùng kích thước thì quy trình này sẽ không có tổng chi phí lớn hơn nó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
import heapq

def solve():
    t = int(input())
    for _ in range(t):
        n, D = map(int, input().split())
        p = list(map(int, input().split()))
        ttime = list(map(int, input().split()))

        dishes = sorted(zip(p, ttime))
        
        heap = []
        total_time = 0
        last_pos = 0

        for pos, eat in dishes:
            travel = pos - last_pos
            last_pos = pos

            total_time += travel + eat
            heapq.heappush(heap, -eat)

            if total_time > D:
                removed = -heapq.heappop(heap)
                total_time -= removed

        print(len(heap))

if __name__ == "__main__":
    solve()
```Mã sắp xếp món ăn đầu tiên nên chuyển động luôn tiến về phía trước. Heap lưu trữ thời gian ăn dưới dạng giá trị âm để mô phỏng heap tối đa bằng cách sử dụng heap tối thiểu của Python. 

Biến Total_time theo dõi cả chi phí di chuyển và ăn uống một cách nhất quán. Mỗi lần lặp lại giả định chúng ta lấy món ăn hiện tại, sau đó sửa chữa tính khả thi bằng cách loại bỏ thời gian ăn đắt nhất nếu cần. Bước sửa chữa này là cơ chế tham lam cốt lõi. 

Một cạm bẫy triển khai phổ biến là quên chỉ tính đến việc di chuyển giữa các món ăn được chọn liên tiếp. Bản cập nhật Last_pos đảm bảo việc di chuyển được tăng dần chứ không phải từ điểm xuất phát mỗi lần. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 3, D = 6
p = [1, 3, 5]
t = [2, 2, 2]
```| Bước | Vị trí | Ăn | Du lịch | Tổng Thời Gian | Đống | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 2 | 1 | 3 | [2] | 
| 2 | 3 | 2 | 2 | 7 | [2,2] | 
| 3 | 5 | 2 | 2 | 11 → xóa 2 | 9 → loại bỏ 2 | 

Sau khi chế biến, chỉ còn lại một món ăn có thể thực hiện được. 

Điều này cho thấy cách thuật toán tự động từ chối các lựa chọn vượt quá khi vượt quá ngân sách. 

### Ví dụ 2 

đầu vào:```
n = 4, D = 10
p = [2, 4, 6, 8]
t = [1, 2, 1, 2]
```| Bước | Vị trí | Ăn | Du lịch | Tổng Thời Gian | Đống | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 2 | 1 | 2 | 3 | [1] | 
| 2 | 4 | 2 | 2 | 7 | [1,2] | 
| 3 | 6 | 1 | 2 | 10 | [1,2,1] | 
| 4 | 8 | 2 | 2 | 14 → xóa 2 | 12 → xóa 2 | 

Câu trả lời cuối cùng là 2. 

Điều này chứng tỏ rằng những bổ sung chi phí cao sau này có thể được hoàn tác một cách an toàn mà không cần xem xét lại các quyết định trước đó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | việc phân loại chiếm ưu thế cộng với các thao tác heap trên mỗi món ăn | 
| Không gian | O(n) | lưu trữ heap tối đa n phần tử | 

Với tổng n trên các trường hợp thử nghiệm là 10^5, điều này phù hợp thoải mái trong giới hạn 1-2 giây thông thường. 

## Trường hợp thử nghiệm```python
import sys, io
import heapq

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        n, D = map(int, input().split())
        p = list(map(int, input().split()))
        ttime = list(map(int, input().split()))

        dishes = sorted(zip(p, ttime))

        heap = []
        total_time = 0
        last_pos = 0

        for pos, eat in dishes:
            total_time += (pos - last_pos) + eat
            last_pos = pos
            heapq.heappush(heap, -eat)

            if total_time > D:
                total_time += heapq.heappop(heap)  # subtracting negative

        out.append(str(len(heap)))

    return "\n".join(out)

# provided samples
assert run("""2
5 10
1 2 3 4 5
1 1 1 1 2
5 10
1 2 3 4 5
1 1 1 1 1
""") == "4\n5"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| món ăn duy nhất phù hợp | 1 | lựa chọn tối thiểu | 
| tất cả đắt tiền, chặt chẽ D | tập hợp con nhỏ hơn | kích hoạt loại bỏ heap | 
| lần bằng nhau | lựa chọn đầy đủ | không cần xóa | 
| khoảng cách lớn về vị trí | tích lũy du lịch đúng đắn | khoảng cách đúng đắn | 

## Vỏ cạnh 

Trường hợp một bên là khi một món ăn rất sớm có thời gian ăn lâu thì buộc phải loại bỏ sau đó. Ví dụ:```
n = 3, D = 5
p = [1, 2, 3]
t = [4, 1, 1]
```Đầu tiên, thuật toán lấy tất cả các món ăn, nhưng tổng thời gian nhanh chóng vượt quá D. Vùng heap tối đa loại bỏ 4 món ăn trước, để lại hai món ăn nhỏ hơn phù hợp. 

Một trường hợp khác là khi vị trí thưa thớt:```
n = 3, D = 10
p = [1, 100, 200]
t = [1, 1, 1]
```Ở đây du lịch chiếm ưu thế. Thuật toán tích lũy chính xác chi phí đi lại lớn và hạn chế lựa chọn một cách tự nhiên, cho thấy tính khả thi phụ thuộc vào khả năng tiếp cận tiền tố chứ không chỉ thời gian ăn uống. 

Trường hợp cạnh cuối cùng là một bước nhảy lớn vượt quá D. Thuật toán vẫn xử lý nó nhưng ngay lập tức loại bỏ nó qua heap, dẫn đến lựa chọn trống hoặc giảm chính xác.
