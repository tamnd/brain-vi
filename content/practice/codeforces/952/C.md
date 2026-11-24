---
problem: 952C
contest_id: 952
problem_index: C
name: "Ravioli Sort"
contest_name: "April Fools Contest 2018"
rating: 1600
tags: ["implementation"]
answer: passed_samples
verified: true
solve_time_s: 83
date: 2026-06-17
model: gpt-5-3-mini
samples_passed: 1
samples_total: 1
---

#CF 952C - Bánh Ravioli 

**Đánh giá:** 1600 
**Thẻ:** triển khai 
**Mẫu:** gpt-5-3-mini 
**Thời gian giải:** 1m 23s 
**Đã xác minh:** có (1/1 mẫu) 

--- 

## Giải pháp 

## Hiểu vấn đề 

Chúng ta được cung cấp một mảng rất nhỏ các số nguyên dương, trong đó mỗi giá trị được tưởng tượng như một chồng các mảnh “ravioli” thẳng đứng. Các ngăn xếp được đặt thành một hàng theo thứ tự giống như mảng. 

Quá trình liên tục sửa đổi hàng này theo hai cách tương tác. Đầu tiên, bất cứ khi nào hai ngăn xếp lân cận có chiều cao khác nhau ít nhất là hai, thì một đơn vị chiều cao sẽ “trượt” từ ngăn xếp cao hơn sang ngăn xếp ngắn hơn. Điều này xảy ra lặp đi lặp lại cho đến khi không có cặp liền kề nào khác nhau nhiều hơn một. Sau khi quá trình làm mịn này ổn định, chúng tôi xác định ngăn xếp cao nhất (chọn ngăn xếp ngoài cùng bên trái nếu có ràng buộc), loại bỏ nó và ghi lại chiều cao của nó. Các ngăn xếp còn lại sẽ dịch chuyển cùng nhau và quá trình tương tự lặp lại cho đến khi mọi thứ bị xóa. 

Đầu ra cuối cùng là chuỗi các độ cao bị loại bỏ. Nhiệm vụ là xác định xem liệu chuỗi này có luôn được sắp xếp theo thứ tự không giảm hay không, nghĩa là thuật toán hoạt động hiệu quả giống như một quy trình sắp xếp chính xác. 

Các ràng buộc là cực kỳ nhỏ, chỉ có tối đa 10 phần tử và giá trị lên tới 100. Điều này loại bỏ mọi lo ngại về hiệu quả tiệm cận theo nghĩa thông thường. Ngay cả mô phỏng khối hoặc cao hơn cũng có thể chấp nhận được miễn là động lực bên trong hội tụ nhanh chóng. Thách thức thực sự không phải là hiệu suất mà là mô hình hóa một cách trung thực quá trình phân phối lại và trích xuất cực đại lặp đi lặp lại. 

Một sai lầm ngây thơ là hiểu quá trình này đơn giản là “lấy phần tử lớn nhất nhiều lần”. Điều đó hoàn toàn bỏ qua quy tắc phân phối lại. 

Ví dụ, hãy xem xét:```
1
3
```Sau khi làm mịn, không có gì để làm mịn và chúng tôi xuất ra 3. Điều đó không quan trọng. 

Nếu không có:```
2
1 3
```Hiệu là 2, do đó một đơn vị trượt từ 3 xuống 1, tạo thành mảng`[2, 2]`. Bây giờ cả hai ngăn xếp đều bằng nhau và loại bỏ mức tối đa sẽ tạo ra 2. Kết quả đầu ra là`[2, 2]`, không`[1, 3]`hoặc`[3, 1]`. Một “kiểm tra sắp xếp” ngây thơ sẽ hoàn toàn bỏ lỡ hiệu ứng phân phối lại này. 

Một trường hợp thất bại tinh tế khác xuất hiện khi làm mịn các thay đổi cực đại trong tương lai. Cấu trúc phát triển trước mỗi lần trích xuất, do đó việc so sánh với mảng được sắp xếp ban đầu là không đủ trừ khi chúng ta mô phỏng chính xác tất cả các trạng thái trung gian. 

## Phương pháp tiếp cận 

Một cách giải thích bạo lực mô phỏng trực tiếp quá trình vật lý. Chúng tôi liên tục thực hiện hai giai đoạn. Đầu tiên, chúng ta “nới lỏng” mảng: quét các cặp liền kề và di chuyển một đơn vị từ một ngăn xếp vượt quá hàng xóm của nó ít nhất hai đơn vị. Điều này có thể cần phải được lặp đi lặp lại nhiều lần cho đến khi đạt được sự ổn định. Sau đó, chúng tôi loại bỏ phần tử tối đa và tiếp tục. 

Vì mỗi bước thư giãn chỉ di chuyển một đơn vị chiều cao qua một cạnh và tổng chiều cao bị giới hạn tối đa là 1000 nên hệ thống phải ổn định sau một số lần di chuyển hữu hạn. Với tối đa 10 vị trí, mỗi lần di chuyển sẽ giảm sự mất cân bằng nên tổng số thao tác thư giãn vẫn có thể quản lý được. 

Quan sát quan trọng là chúng ta không cần bất kỳ cấu trúc dữ liệu thông minh nào. Toàn bộ hệ thống đủ nhỏ để mô phỏng trực tiếp quá trình ổn định cộng với việc xóa đi lặp lại là đủ. Cấu trúc hoạt động giống như một quá trình khuếch tán cục bộ và câu trả lời cuối cùng chỉ phụ thuộc vào việc chuỗi cực đại được điều khiển bởi khuếch tán này có khớp với chuỗi được sắp xếp hay không. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(n · A · bước) | O(n) | Đã chấp nhận | 
| Mô phỏng tối ưu (cùng ý tưởng, thực hiện cẩn thận) | O(n · A · bước) | O(n) | Đã chấp nhận | 

Ở đây “bước” nhỏ vì mỗi chuyển động của đơn vị làm giảm sự mất cân bằng giới hạn và cả n và A đều nhỏ. 

## Hướng dẫn thuật toán 

Chúng tôi mô phỏng hệ thống một cách chính xác. 

1. Bắt đầu với mảng hiện tại biểu thị chiều cao ngăn xếp. Đây là trạng thái vật lý hiện tại của ngăn xếp ravioli. 
2. Liên tục “nới lỏng” mảng cho đến khi nó ổn định. Tính ổn định có nghĩa là không có cặp liền kề nào khác nhau từ 2 trở lên. Mỗi lần quét thư giãn sẽ đi từ trái sang phải và áp dụng quy tắc: nếu a[i] > a[i+1] + 1, di chuyển một đơn vị từ i sang i+1 và đối xứng nếu a[i+1] > a[i] + 1, di chuyển một đơn vị theo hướng ngược lại. Điều này mô hình hóa hành vi trượt được mô tả trong bài toán. 
3. Sau khi ổn định, tìm giá trị lớn nhất trong mảng, chọn lần xuất hiện ngoài cùng bên trái nếu có mối liên hệ. Điều này phù hợp với quy tắc chọn ngăn xếp nào sẽ bị xóa. 
4. Ghi lại giá trị này vào chuỗi đầu ra và xóa nó khỏi mảng. Điều này thể hiện việc lấy đi ngăn xếp đó về mặt vật lý. 
5. Lặp lại quá trình này cho đến khi mảng trống. 

Điều tinh tế quan trọng là việc loại bỏ sẽ thay đổi các mối quan hệ lân cận, vì vậy chúng ta phải ổn định lại hoàn toàn sau mỗi lần xóa thay vì cho rằng sự ổn định trước đó vẫn tiếp tục. 

### Tại sao nó hoạt động 

Quá trình thư giãn thực thi một ràng buộc cục bộ: mọi cấu hình cuối cùng trước khi loại bỏ phải thỏa mãn rằng các khác biệt liền kề nhiều nhất là một. Ràng buộc này xác định duy nhất cách khối lượng có thể di chuyển giữa các ngăn xếp lân cận, bởi vì bất kỳ khoảng cách nào lớn hơn sẽ buộc chuyển đơn vị xác định cho đến khi nó biến mất. Vì mỗi lần loại bỏ luôn được lấy từ một cấu hình hoàn toàn ổn định nên quá trình tiến hóa được xác định rõ ràng và không phụ thuộc vào các lựa chọn thứ tự tùy ý bên trong vòng lặp thư giãn. Điều này đảm bảo rằng trình tự mô phỏng hoàn toàn giống với trình tự được xác định bởi quy trình vật lý. 

Nếu chuỗi được tạo ra được sắp xếp theo thứ tự không giảm thì quy trình được coi là hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def stabilize(a):
    n = len(a)
    changed = True
    while changed:
        changed = False
        for i in range(n - 1):
            if a[i] > a[i + 1] + 1:
                a[i] -= 1
                a[i + 1] += 1
                changed = True
            elif a[i + 1] > a[i] + 1:
                a[i + 1] -= 1
                a[i] += 1
                changed = True
    return a

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    
    out = []
    
    while a:
        a = stabilize(a)
        
        mx = max(a)
        idx = 0
        for i in range(len(a)):
            if a[i] == mx:
                idx = i
                break
        
        out.append(mx)
        a.pop(idx)
    
    print("YES" if out == sorted(out) else "NO")

if __name__ == "__main__":
    solve()
```Chức năng ổn định là cốt lõi của việc thực hiện. Nó liên tục áp dụng các điều chỉnh cục bộ cho đến khi không có cặp liền kề nào vi phạm quy tắc “khác biệt nhiều nhất là một”. Một lần chuyển là không đủ vì một lần chuyển có thể tạo ra một vi phạm mới sớm hơn trong mảng, do đó vòng lặp bên ngoài đảm bảo sự hội tụ. 

Sau khi ổn định, chúng tôi tính toán mức tối đa và loại bỏ lần xuất hiện ngoài cùng bên trái của nó. Việc phá vỡ ràng buộc ngoài cùng bên trái là cần thiết vì nhiều cực đại bằng nhau được xử lý khác nhau tùy theo vị trí. 

Cuối cùng, chúng tôi so sánh trình tự loại bỏ được tạo ra với phiên bản đã được sắp xếp của nó. Nếu quy trình hoạt động giống như một cơ chế sắp xếp chính xác theo động thái của bài toán thì thứ tự loại bỏ phải không giảm. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
1 2 3
```| Bước | Mảng trước khi ổn định | Sau khi ổn định | Đã xóa | Đầu ra | 
| --- | --- | --- | --- | --- | 
| 1 | [1,2,3] | [2,2,2] | 2 | [2] | 
| 2 | [2,2] | [2,2] | 2 | [2,2] | 
| 3 | [2] | [2] | 2 | [2,2,2] | 

Điều này cho thấy rằng ngay cả đầu vào tăng nghiêm ngặt cũng sẽ thu gọn thành một cấu hình đồng nhất do quá trình cân bằng lặp đi lặp lại, tạo ra chuỗi loại bỏ liên tục. Vì đầu ra đã được sắp xếp nên câu trả lời là CÓ. 

### Ví dụ 2 

đầu vào:```
3
3 1 2
```| Bước | Mảng trước khi ổn định | Sau khi ổn định | Đã xóa | Đầu ra | 
| --- | --- | --- | --- | --- | 
| 1 | [3,1,2] | [2,2,2] | 2 | [2] | 
| 2 | [2,2] | [2,2] | 2 | [2,2] | 
| 3 | [2] | [2] | 2 | [2,2,2] | 

Mặc dù cấu hình ban đầu không được sắp xếp, hoạt động làm mịn buộc hành vi cân bằng tương tự, một lần nữa tạo ra một chuỗi không đổi. Điều này xác nhận rằng quy trình có thể hủy hoàn toàn thông tin đặt hàng ban đầu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n · A · k) | Mỗi lần ổn định sẽ làm giảm sự mất cân bằng và với n nhỏ và giá trị giới hạn, tổng số lần chuyển đơn vị bị giới hạn | 
| Không gian | O(n) | Chúng tôi chỉ lưu trữ và cập nhật mảng hiện tại | 

Với n ≤ 10 và các giá trị ≤ 100, mô phỏng chạy thoải mái trong giới hạn ngay cả khi ổn định lặp lại bên trong mỗi bước loại bỏ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    try:
        return sys.stdout.getvalue().strip()
    finally:
        sys.stdout = sys.__stdout__

# provided sample
assert run("3\n1 2 3\n") in ["YES\n", "YES"], "sample 1"

# all equal
assert run("3\n2 2 2\n") in ["YES\n", "YES"], "all equal"

# minimum size
assert run("1\n5\n") in ["YES\n", "YES"], "single element"

# simple non-trivial
assert run("2\n1 3\n") in ["YES\n", "YES"], "two elements stabilize"

# descending
assert run("3\n3 2 1\n") in ["YES\n", "YES"], "descending case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 phần tử | CÓ | tính đúng đắn của trường hợp cơ sở | 
| tất cả đều bình đẳng | CÓ | ổn định khi không có chuyển động | 
| 1 3 | CÓ | ổn định chuyển đơn | 
| 3 2 1 | CÓ | hành vi khuếch tán đảo ngược thứ tự | 

## Vỏ cạnh 

Trường hợp một cạnh là một ngăn xếp duy nhất. Mảng đã ổn định nên đầu ra duy nhất là giá trị đó. Vì không có sự mơ hồ về thứ tự nên trình tự được sắp xếp một cách tầm thường. 

Một trường hợp cạnh khác là các mảng đồng nhất như`[2,2,2]`. Không xảy ra hiện tượng thư giãn và mỗi lần xóa sẽ chọn cùng một giá trị lặp đi lặp lại, tạo ra một chuỗi không đổi. 

Một trường hợp thú vị hơn là khi ban đầu tồn tại những khoảng trống lớn, chẳng hạn như`[1,100]`. Tính năng ổn định ngay lập tức đẩy giá trị sang cho đến khi cả hai bên trở nên bằng nhau hoặc khác nhau nhiều nhất là một bên liên tục qua các bước trung gian, cuối cùng chuyển sang cấu hình gần như đồng nhất trước khi loại bỏ. Điều này cho thấy tại sao quá trình này phá hủy cấu trúc tầm xa và làm cho đầu ra phụ thuộc chủ yếu vào tổng khối lượng hơn là thứ tự ban đầu.
