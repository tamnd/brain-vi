---
title: "CF 105986K - Ngăn xếp của Capoo"
description: "Chúng tôi được cung cấp nhiều trường hợp thử nghiệm độc lập. Trong mỗi trường hợp thử nghiệm có một số Capoo, mỗi Capoo có giá trị cường độ dương. Chúng tôi muốn chọn một số trong số chúng và sắp xếp chúng thành một chồng dọc. Ngăn xếp có một hạn chế là chỉ Capoo hàng đầu là không bị hạn chế."
date: "2026-06-22T16:35:19+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105986
codeforces_index: "K"
codeforces_contest_name: "2025 Wuhan University of Technology Programming Contest"
rating: 0
weight: 105986
solve_time_s: 60
verified: true
draft: false
---

[CF 105986K - Ngăn xếp của Capoo](https://codeforces.com/problemset/problem/105986/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp nhiều trường hợp thử nghiệm độc lập. Trong mỗi trường hợp thử nghiệm có một số Capoo, mỗi Capoo có giá trị cường độ dương. Chúng tôi muốn chọn một số trong số chúng và sắp xếp chúng thành một chồng dọc. 

Ngăn xếp có một hạn chế là chỉ Capoo hàng đầu là không bị hạn chế. Mọi Capoo khác phải có sức mạnh ít nhất bằng tổng sức mạnh của tất cả các Capoo được đặt phía trên nó. Nói cách khác, khi chúng ta đi xuống trong ngăn xếp, mỗi Capoo phải đủ mạnh để “hỗ trợ” mọi thứ phía trên nó cộng lại. 

Mục tiêu không phải là tối đa hóa sức mạnh tổng thể hoặc giảm thiểu vi phạm mà là tối đa hóa số lượng Capoo có thể được đưa vào trong khi vẫn có thể sắp xếp chúng thành một ngăn xếp hợp lệ. 

Từ góc độ ràng buộc, tổng số Capoo trong tất cả các trường hợp thử nghiệm tối đa là 200000 và mỗi cường độ có thể lớn tới 10^9. Điều này ngay lập tức loại trừ mọi cách tiếp cận thử tất cả các hoán vị hoặc tập hợp con. Ngay cả việc kiểm tra một cách sắp xếp đơn lẻ cũng cần phải xác minh tuyến tính và có rất nhiều thứ tự theo giai thừa, do đó, vũ lực hoàn toàn nằm ngoài tầm với. 

Khó khăn chính là cả việc lựa chọn và đặt hàng đều được kết hợp với nhau. Việc chọn sớm phần tử yếu có thể khiến tổng tiền tố trở nên quá lớn và nhanh chóng, ngăn cản việc bổ sung sau này. Mặt khác, việc đặt các phần tử mạnh quá sớm có thể lãng phí dung lượng của chúng và giảm số lượng vật phẩm phù hợp về tổng thể. 

Trường hợp cạnh tinh tế xuất hiện khi nhiều giá trị bằng nhau hoặc rất nhỏ. Ví dụ: nếu tất cả Capoo đều có sức mạnh 1 thì xếp hai cũng được, nhưng xếp ba đã thất bại vì cái thứ ba phải có ít nhất là 2. Vì vậy, cách tiếp cận ngây thơ “lấy mọi thứ” hoặc “sắp xếp tùy ý” sẽ phá vỡ ngay lập tức. Một trường hợp phức tạp khác là khi tồn tại một giá trị rất lớn giữa nhiều giá trị nhỏ. Ví dụ: các giá trị như [1, 1, 1, 100]. Nếu được đặt không chính xác, giá trị lớn có thể bị tiêu tốn quá sớm, khiến cho sau đó không thể lắp thêm các phần tử nhỏ hơn mặc dù đã có thứ tự tốt hơn. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là thử mọi tập hợp con của Capoo và với mỗi tập hợp con hãy thử tất cả các hoán vị để xem liệu có tồn tại một ngăn xếp hợp lệ hay không. Đối với một thứ tự nhất định, chúng ta có thể xác minh tính hợp lệ theo thời gian tuyến tính bằng cách duy trì tổng các phần tử phía trên mỗi vị trí. Điều này đã tốn O(k) cho mỗi hoán vị và trên tất cả các hoán vị, giá trị này trở thành O(k!), điều này không thể thực hiện được ngay cả với n khoảng 15, chứ chưa nói đến 200000. 

Chúng ta cần một quan sát mang tính cấu trúc về cách hoạt động của ràng buộc. Điều kiện chỉ phụ thuộc vào tổng của tất cả các phần tử được đặt trước đó chứ không phụ thuộc vào sự sắp xếp riêng lẻ của chúng. Điều này gợi ý rằng sau khi chúng tôi sửa thứ tự, trạng thái duy nhất quan trọng trong quá trình xây dựng là tổng tiền tố hiện tại. 

Điều này dẫn đến một cái nhìn sâu sắc tham lam: nếu chúng ta đang xây dựng một ngăn xếp hợp lệ từ trên xuống dưới, ở mỗi bước, chúng ta muốn giữ tổng tiền tố càng nhỏ càng tốt để có nhiều phần tử đủ điều kiện được đặt sau này. Điều này tự nhiên gợi ý việc sắp xếp và luôn thử phần tử hợp lệ nhỏ nhất có sẵn trước tiên. 

Nhận thức quan trọng là nếu chúng ta duy trì tổng S hiện tại của mọi thứ đã được đặt thì phần tử được chọn tiếp theo phải thỏa mãn ai ≥ S. Trong số tất cả các ứng cử viên hợp lệ như vậy, việc chọn giá trị nhỏ nhất sẽ giữ cho S càng nhỏ càng tốt sau khi cập nhật, vì S trở thành S + ai. Đây chính xác là cấu trúc của một quá trình khả thi tham lam. 

Vì vậy, chúng tôi sắp xếp tất cả các giá trị theo thứ tự tăng dần và lặp qua chúng, tham lam lấy một phần tử bất cứ khi nào nó có thể hỗ trợ tổng hiện tại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Ồ (n!) | O(n) | Quá chậm | 
| Tham lam sau khi phân loại | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng trường hợp thử nghiệm một cách độc lập. 

1. Sắp xếp tất cả điểm mạnh của Capoo theo thứ tự tăng dần.

Việc sắp xếp đảm bảo rằng khi chúng ta xem xét một phần tử, tất cả các phần tử nhỏ hơn hoặc bằng nhau đều đã được xem xét, điều này rất quan trọng vì các phần tử nhỏ hơn luôn dễ dàng được đặt sớm hơn mà không vi phạm ràng buộc về tổng. 
2. Khởi tạo biến S = 0 để biểu thị tổng các điểm mạnh đã được đặt trong ngăn xếp. 

Giá trị này mô hình tổng tải mà Capoo được chọn tiếp theo phải có khả năng hỗ trợ. 
3. Khởi tạo bộ đếm ans = 0 để theo dõi xem chúng ta đã thêm thành công bao nhiêu Capoo. 
4. Lặp lại các điểm mạnh được sắp xếp từ nhỏ nhất đến lớn nhất. Với mỗi giá trị x: 

Nếu x ít nhất là S, chúng ta đưa nó vào ngăn xếp, tăng ans lên 1 và cập nhật S = S + x. 

Nếu x nhỏ hơn S thì bỏ qua vì đặt vào sẽ vi phạm yêu cầu ngay. Không có lợi ích gì khi cố gắng ép buộc nó sau này, vì S chỉ tăng theo thời gian. 
5. Sau khi xử lý tất cả các phần tử, xuất ra kết quả là số lượng Capoo tối đa có thể xếp chồng lên nhau. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào, cách duy nhất để mở rộng ngăn xếp hợp lệ là chọn một phần tử có thể hỗ trợ tải tích lũy hiện tại S. Nếu chúng tôi chọn phần tử lớn hơn mức cần thiết khi tồn tại phần tử hợp lệ nhỏ hơn, chúng tôi sẽ tăng S nhiều hơn mức cần thiết, điều này chỉ có thể làm giảm tính khả thi trong tương lai vì tất cả các phần tử còn lại đều cố định và không âm. 

Điều này thiết lập một thuộc tính thống trị tham lam: trong số tất cả các lựa chọn hợp lệ ở một bước nhất định, phần tử hợp lệ nhỏ nhất sẽ bảo toàn các tùy chọn trong tương lai nhất. Vì S tăng đơn điệu nên việc bỏ qua một phần tử nhỏ hơn hợp lệ không bao giờ có ích sau này, vì nó chỉ làm cho ràng buộc chặt chẽ hơn đối với mọi phần tử còn lại. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        
        a.sort()
        
        s = 0
        ans = 0
        
        for x in a:
            if x >= s:
                ans += 1
                s += x
        
        out.append(str(ans))
    
    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Giải pháp được cấu trúc xung quanh một lần quét tham lam duy nhất. Bước sắp xếp đảm bảo rằng trước tiên chúng ta luôn gặp các ứng cử viên nhỏ hơn, điều này rất quan trọng để giữ số tiền tích lũy ở mức tối thiểu bất cứ khi nào có thể. Biến`s`theo dõi yêu cầu cường độ tích lũy do tiền tố đã chọn của ngăn xếp gây ra. Mỗi quyết định đều mang tính cục bộ: chúng tôi chấp nhận một phần tử nếu nó hiện có thể hỗ trợ ngăn xếp hoặc bỏ qua phần tử đó vĩnh viễn. 

Một lỗi phổ biến là cố gắng xây dựng ngăn xếp theo thứ tự giảm dần. Cách tiếp cận đó có xu hướng đặt các giá trị lớn sớm, làm tăng tổng số hiện có và giảm số lượng phần tử có thể phù hợp sau này. Một sai lầm khác là thử ghép nối các phần tử hoặc sử dụng hai con trỏ mà không thực thi ràng buộc tổng tiền tố toàn cục, điều này dẫn đến suy luận tính khả thi không chính xác. 

## Ví dụ đã hoạt động 

Hãy xem xét đầu vào:```
1
3
4 7 9
```Sau khi sắp xếp, ta được [4, 7, 9]. 

| Bước | Hiện tại x | S hiện tại | Hành động | S mới | Trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 4 | 0 | lấy | 4 | 1 | 
| 2 | 7 | 4 | lấy | 11 | 2 | 
| 3 | 9 | 11 | bỏ qua | 11 | 2 | 

Điều này cho thấy rằng mặc dù số 9 lớn nhưng nó đến quá muộn vì tổng tích lũy trở nên quá lớn để nó có thể thỏa mãn ràng buộc. 

Bây giờ hãy xem xét:```
1
3
1 1 1
```Mảng được sắp xếp là [1, 1, 1]. 

| Bước | Hiện tại x | S hiện tại | Hành động | S mới | Trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 0 | lấy | 1 | 1 | 
| 2 | 1 | 1 | lấy | 2 | 2 | 
| 3 | 1 | 2 | bỏ qua | 2 | 2 | 

Chỉ có thể sử dụng hai phần tử vì số 1 thứ ba không thể hỗ trợ tổng tích lũy của 2. 

Những dấu vết này cho thấy ràng buộc nhanh chóng trở nên chặt chẽ hơn như thế nào khi S tăng lên, đây là yếu tố hạn chế cốt lõi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) cho mỗi trường hợp thử nghiệm | Sắp xếp chiếm ưu thế, quét tuyến tính đơn sau đó | 
| Không gian | O(1) bổ sung (không bao gồm đầu vào) | Chỉ có một vài biến được sử dụng ngoài khả năng lưu trữ của mảng | 

Tổng độ phức tạp của tất cả các trường hợp thử nghiệm là O(N log N), trong đó N là tổng của tất cả n giá trị. Với N lên tới 2 × 10^5, điều này dễ dàng phù hợp với giới hạn thông thường. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        a.sort()
        s = 0
        ans = 0
        for x in a:
            if x >= s:
                ans += 1
                s += x
        out.append(str(ans))
    return "\n".join(out)

# provided samples
assert run("1\n3\n4 7 9\n") == "2"
assert run("1\n3\n1 1 1\n") == "2"

# custom cases
assert run("1\n1\n5\n") == "1", "single element"
assert run("1\n5\n1 2 3 4 5\n") == "3", "mixed increasing"
assert run("1\n4\n10 1 1 1\n") == "3", "large + smalls"
assert run("1\n6\n1 1 1 1 1 10\n") == "3", "late large element"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 phần tử | 1 | độ chính xác kích thước tối thiểu | 
| 1 2 3 4 5 | 3 | hành vi tiến bộ tham lam | 
| 10 1 1 1 | 3 | ảnh hưởng của hạn chế sớm lớn | 
| nhiều số 1 + 10 | 3 | độ nhạy đặt hàng | 

## Vỏ cạnh 

Một trường hợp cạnh quan trọng là khi tất cả các giá trị đều bằng 1. Đối với đầu vào`[1, 1, 1, 1]`, thuật toán lấy phần tử đầu tiên, tạo thành S = 1. Phần tử thứ hai vẫn hợp lệ, cho S = 2. Phần tử 1 thứ ba sau đó bị từ chối vì nó không thể hỗ trợ 2. Điều này chứng tỏ rằng ngay cả các mảng thống nhất cũng không cho phép lựa chọn đầy đủ do tổng tiền tố tăng nhanh. 

Một trường hợp cạnh khác là một phần tử lớn được bao quanh bởi nhiều phần tử nhỏ, chẳng hạn như`[100, 1, 1, 1, 1]`. Thuật toán đầu tiên lấy 1, rồi 1, rồi 1, nhưng cuối cùng S phát triển và chặn thêm sự đưa vào. Phần tử lớn thường được thực hiện muộn nếu có và việc sắp xếp này đảm bảo chúng tôi không lãng phí công suất sớm cho phần tử đó. 

Trường hợp cạnh cuối cùng là đầu vào một phần tử. Vì phần tử trên cùng không có ràng buộc nào phía trên nó nên bất kỳ Capoo đơn lẻ nào cũng luôn hợp lệ và thuật toán trả về chính xác 1 ngay lập tức.
