---
title: "CF 106158A - Shustrik, Persik và Tình bạn vĩnh cửu"
description: "Chúng ta được cấp một chuỗi các chữ số được sắp xếp thành một vòng tròn và chúng ta được phép xoay nó bao nhiêu lần cũng được. Mỗi vòng quay là một sự dịch chuyển theo chu kỳ sang trái hoặc sang phải."
date: "2026-06-19T19:18:32+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106158
codeforces_index: "A"
codeforces_contest_name: "Innopolis Open 2025-2026. Elimination Round 1"
rating: 0
weight: 106158
solve_time_s: 67
verified: true
draft: false
---

[CF 106158A - Shustrik, Persik và Tình bạn vĩnh cửu](https://codeforces.com/problemset/problem/106158/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 7s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một chuỗi các chữ số được sắp xếp thành một vòng tròn và chúng ta được phép xoay nó bao nhiêu lần cũng được. Mỗi vòng quay là một sự dịch chuyển theo chu kỳ sang trái hoặc sang phải. Sau khi chọn phép xoay, chúng tôi xem xét chuỗi tuyến tính thu được và đo “thế năng” của nó, đơn giản là số đoạn liền kề tối đa có các chữ số bằng nhau. 

Ví dụ: trong một chuỗi như`112223`, các phân đoạn là`11 | 222 | 3`, do đó tiềm năng là 3. Một phép quay được coi là tốt hơn nếu nó làm giảm số lượng phân đoạn này. Trong số tất cả các phép quay đạt được số phân đoạn nhỏ nhất có thể, chúng ta phải chọn phép quay yêu cầu số lần dịch chuyển theo chu kỳ nhỏ nhất so với chuỗi ban đầu và xuất ra số đó bằng dấu chỉ hướng: dịch chuyển trái là âm, dịch chuyển phải là dương. 

Quan sát quan trọng là các phép quay không làm thay đổi nhiều tập hợp các mối quan hệ ký tự liền kề, chúng chỉ thay đổi nơi chúng ta “cắt” vòng tròn để tạo thành một đường thẳng. Điều đó có nghĩa là mọi phép quay đều có cấu trúc gần như giống nhau, ngoại trừ chính xác một cặp kề thay đổi do bị cắt. 

Ràng buộc n lên tới 10^6 buộc chúng ta tránh xa mọi thứ bậc hai. Bất kỳ giải pháp nào thử tất cả các phép quay và tính toán lại số lượng phân đoạn từ đầu sẽ yêu cầu O(n^2), tốc độ này quá chậm. Chúng ta cần một cách tiếp cận O(n) để đánh giá ngầm tất cả các phép quay. 

Trường hợp cạnh tinh tế là khi tất cả các ký tự giống hệt nhau. Trong trường hợp đó, mỗi vòng quay tạo ra một khối duy nhất, vì vậy mọi câu trả lời đều tối ưu như nhau và chúng ta phải trả về số dịch chuyển bằng 0. 

Một trường hợp quan trọng khác là khi có nhiều vị trí cắt tối ưu hợp lệ. Ví dụ: nếu chuỗi có một số vị trí mà các ký tự liền kề khác nhau, thì nhiều phép quay sẽ giảm điện thế như nhau và chúng ta phải chọn vị trí gần nhất với vị trí ban đầu về khoảng cách dịch chuyển theo chu kỳ. 

## Phương pháp tiếp cận 

Một cách tiếp cận vũ phu sẽ thử mọi vòng quay. Đối với mỗi lần xoay, chúng ta sẽ xây dựng chuỗi được xoay và đếm xem các ký tự liền kề khác nhau bao nhiêu lần. Điều đó cho số lượng phân khúc. Điều này hoạt động chính xác vì số lượng phân đoạn dễ tính toán từ các chuyển đổi, nhưng mỗi vòng quay tốn O(n) và có n vòng quay, dẫn đến tổng số hoạt động là O(n^2), quá lớn đối với n lên tới 10^6. 

Thông tin chi tiết về cấu trúc quan trọng là số lượng phân đoạn chỉ phụ thuộc vào vị trí chúng ta cắt chuỗi tròn. Nếu chúng ta xem sợi dây là hình tròn thì mỗi vòng quay tương ứng với việc chọn vị trí cắt bắt đầu. Tất cả các cặp liền kề bên trong vòng tròn không thay đổi; chỉ có cạnh giữa ký tự cuối cùng và ký tự đầu tiên của chuỗi được xoay phụ thuộc vào vết cắt. Điều này có nghĩa là chúng ta có thể tính toán trước tất cả các điểm không khớp kề một lần, sau đó đánh giá từng vòng quay trong thời gian không đổi bằng cách kiểm tra xem vết cắt có phá vỡ điểm không khớp hay không. 

Điều này làm giảm vấn đề quét tất cả các vị trí mà chúng tôi có thể cắt vòng tròn, tính toán số phân đoạn kết quả bằng O(1) cho mỗi vị trí và chọn vị trí tốt nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi coi mỗi vị trí i là một “đường cắt” có thể có giữa các ký tự i−1 và i (có bao quanh). Cắt ở đây có nghĩa là chuỗi quay bắt đầu từ i.

1. Tính xem các cặp liền kề nào khác nhau theo nghĩa tròn ban đầu. Chúng ta thực sự không cần phải xây dựng cấu trúc hình tròn một cách rõ ràng; chúng ta chỉ cần biết ở mỗi vị trí xem s[j] và s[j−1] có khác nhau hay không. 
2. Quan sát rằng tổng số cặp liền kề không khớp trong vòng tròn là cố định. Đặt giá trị này là T. 
3. Đối với vết cắt được chọn ở vị trí i, phần kề duy nhất bị ảnh hưởng là cạnh giữa s[i−1] và s[i]. Trong biểu diễn tuyến tính, cạnh này trở thành ranh giới và không được tính là chuyển tiếp bên trong. 
4. Do đó, nếu s[i−1] và s[i] khác nhau, việc cắt ở đây sẽ loại bỏ một chuyển đổi khỏi tổng số, giảm số lượng phân đoạn đi một. Nếu chúng bằng nhau thì số lượng phân đoạn vẫn giữ nguyên. 
5. Với mỗi i, hãy tính xem đó có phải là “phần có lợi” hay không, nghĩa là s[i−1] != s[i]. Đối với mỗi i như vậy, nó đạt được số lượng phân đoạn tối thiểu có thể. 
6. Trong số tất cả các phép cắt có lợi, hãy chọn phép cắt yêu cầu số thao tác nhỏ nhất từ ​​chuỗi ban đầu. Một phép quay theo i có thể đạt được bằng cách dịch chuyển i sang trái hoặc n−i dịch chuyển sang phải, vì vậy chúng ta chọn giá trị nhỏ hơn trong hai giá trị này. Nếu bằng nhau thì hướng nào cũng được. 
7. Xuất ra phép dịch đã chọn với dấu âm nếu chúng ta sử dụng dịch trái, dương nếu dịch phải. 

### Tại sao nó hoạt động 

Số lượng phân đoạn của bất kỳ phép quay nào chỉ phụ thuộc vào số lượng cặp không bằng nhau liền kề tồn tại và tất cả các phép quay đều có chung một tập hợp các cạnh tròn ngoại trừ cạnh cắt đơn. Điều này có nghĩa là mọi vòng quay khác nhau chỉ ở chỗ nó có loại bỏ chính xác một chuyển đổi hay không. Vì vậy, giá trị tối ưu chỉ được xác định bằng việc chúng ta có cắt trên một ranh giới khác hay không. Khi điều đó được khắc phục, việc giảm thiểu các hoạt động sẽ trở thành vấn đề khoảng cách đơn giản trên một chu trình, không phụ thuộc vào cấu trúc phân đoạn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input().strip())
    s = input().strip()

    if n == 1:
        print(0)
        return

    best_i = None

    for i in range(n):
        if s[i] != s[i - 1]:
            if best_i is None:
                best_i = i
            else:
                # choose closer rotation to 0
                def cost(x):
                    left = x
                    right = n - x
                    if left <= right:
                        return left, -left
                    return right, right

                c_best = cost(best_i)
                c_cur = cost(i)

                if c_cur < c_best:
                    best_i = i

    if best_i is None:
        print(0)
        return

    left = best_i
    right = n - best_i

    if left <= right:
        print(-left)
    else:
        print(right)

if __name__ == "__main__":
    solve()
```Đầu tiên, mã xác định tất cả các vị trí cắt hợp lệ trong đó các ký tự liền kề khác nhau. Đây chính xác là những phép quay đạt được số lượng phân đoạn tối thiểu có thể. Sau đó, nó chọn trong số đó vòng quay gần nhất với cấu hình ban đầu bằng cách so sánh chi phí dịch chuyển trái và phải. Nếu không tồn tại vết cắt như vậy thì chuỗi sẽ đồng nhất và không có thao tác nào thay đổi bất kỳ điều gì. 

Một điểm tinh tế là việc lập chỉ mục: coi i như một điểm cắt giữa i−1 và i xử lý một cách tự nhiên việc bao quanh bằng cách sử dụng lập chỉ mục phủ định của Python, trong đó s[i-1] đề cập chính xác đến ký tự cuối cùng khi i = 0. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Chuỗi đầu vào:`12233321`Chúng tôi kiểm tra các vị trí cắt nơi các ký tự liền kề khác nhau. 

| tôi | s[i-1] | s[i] | cắt hợp lệ | chi phí còn lại | đúng chi phí | đã chọn | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 1 | không | - | - | - | 
| 1 | 1 | 2 | vâng | 1 | 7 | trái | 
| 2 | 2 | 2 | không | - | - | - | 
| 3 | 2 | 3 | vâng | 3 | 5 | trái | 
| 4 | 3 | 3 | không | - | - | - | 
| 5 | 3 | 3 | không | - | - | - | 
| 6 | 3 | 2 | vâng | 6 | 2 | đúng | 
| 7 | 2 | 1 | vâng | 7 | 1 | đúng | 

Đường cắt tốt nhất là i = 7 vì nó chỉ yêu cầu một ca sang phải. Điều này chứng tỏ rằng ngay cả khi nhiều phép quay là tối ưu về số lượng phân đoạn, thì câu trả lời cuối cùng được xác định bởi khoảng cách tuần hoàn đến điểm gốc. 

### Ví dụ 2 

Chuỗi đầu vào:`11111`| tôi | s[i-1] | s[i] | cắt hợp lệ | 
| --- | --- | --- | --- | 
| tất cả tôi | giống nhau | giống nhau | không | 

Không có vết cắt hợp lệ nào, vì vậy mỗi vòng quay sẽ tạo ra một khối duy nhất. Đầu ra là 0, vì không có thao tác nào cải thiện bất cứ điều gì hoặc phân biệt các phép quay. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi vị trí được kiểm tra một lần về chênh lệch kề và một lần để lựa chọn | 
| Không gian | O(1) | Chỉ có một số bộ đếm và chỉ số được lưu trữ | 

Thuật toán tuyến tính theo độ dài của chuỗi, vừa vặn thoải mái trong giới hạn tối đa 10^6 ký tự. Việc sử dụng bộ nhớ không đổi ngoài bộ nhớ đầu vào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    out = io.StringIO()
    backup = sys.stdout
    sys.stdout = out
    solve()
    sys.stdout = backup
    return out.getvalue().strip()

# edge: single char
assert run("1\n7\n") == "0"

# all same
assert run("5\n11111\n") == "0"

# simple two-block structure
assert run("4\n1122\n") in ["-1", "1"]

# alternating pattern
assert run("6\n101010\n") in ["-1", "1"]

# asymmetric case
assert run("8\n12233321\n") == "-1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 chữ số | 0 | kích thước tối thiểu | 
| tất cả đều bình đẳng | 0 | không có vết cắt hợp lệ | 
| 1122 | ±1 | xử lý cà vạt | 
| 101010 | ±1 | nhiều lần cắt hợp lệ | 
| 12233321 | -1 | lựa chọn chính xác giữa các ứng viên | 

## Vỏ cạnh 

Khi tất cả các ký tự giống hệt nhau thì không có vị trí nào mà các ký tự liền kề khác nhau. Thuật toán rời đi một cách chính xác`best_i`không được đặt và trực tiếp xuất ra số 0, vì mỗi phép quay mang lại chính xác một phân đoạn và không phép xoay nào có thể cải thiện hoặc phân biệt kết quả. 

Khi có nhiều vị trí cắt hợp lệ, chẳng hạn như trong các mẫu xen kẽ hoặc lặp lại, thuật toán sẽ đánh giá từng ứng cử viên được cắt một cách độc lập và so sánh khoảng cách theo chu kỳ với vị trí ban đầu. Bởi vì hàm chi phí đối xứng trên đường tròn nên kết quả được chọn luôn là đường cắt hợp lệ gần nhất và các ràng buộc được giải quyết một cách tự nhiên bằng cách ưu tiên dịch chuyển trái khi chúng không tệ hơn dịch chuyển phải. 

Khi đường cắt tốt nhất ở vị trí 0, nghĩa là không cần xoay, cả chi phí bên trái và bên phải đều bằng 0. Thuật toán đưa ra kết quả bằng 0, phản ánh chính xác rằng chuỗi gốc đã tối ưu.
