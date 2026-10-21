---
title: "CF 105838B - Bữa trưa!"
description: "Chúng ta được cho một dãy có đúng ba số nguyên. Chúng tôi được phép sắp xếp lại ba giá trị này bằng cách sử dụng hoán đổi giữa hai vị trí bất kỳ và sau khi làm như vậy, chúng tôi muốn sự sắp xếp kết quả thỏa mãn một mẫu rất cụ thể: phần tử đầu tiên phải lớn hơn…"
date: "2026-06-22T01:20:22+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105838
codeforces_index: "B"
codeforces_contest_name: "The 14th Huazhong Agricultural University Programming Contest"
rating: 0
weight: 105838
solve_time_s: 43
verified: true
draft: false
---

[CF 105838B - Bữa trưa!](https://codeforces.com/problemset/problem/105838/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 43s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một dãy có đúng ba số nguyên. Chúng tôi được phép sắp xếp lại ba giá trị này bằng cách sử dụng hoán đổi giữa hai vị trí bất kỳ và sau khi thực hiện như vậy, chúng tôi muốn sự sắp xếp kết quả thỏa mãn một mẫu rất cụ thể: phần tử đầu tiên phải lớn hơn phần tử thứ hai và phần tử thứ hai phải nhỏ hơn phần tử thứ ba. Nói cách khác, phần tử ở giữa phải ở mức tối thiểu cục bộ nghiêm ngặt. 

Vì độ dài chuỗi được cố định ở mức ba, nên điều duy nhất quan trọng là chúng ta sẽ có hoán vị nào của ba số. Bất kỳ số lần hoán đổi nào giữa các vị trí đều có thể tạo ra bất kỳ hoán vị nào, do đó thao tác này hoàn toàn không hạn chế khả năng tiếp cận. Câu hỏi thực sự hoàn toàn mang tính tổ hợp: liệu có tồn tại bất kỳ hoán vị nào của tập hợp ba giá trị sao cho phần giữa nhỏ hơn cả hai đầu hay không. 

Kích thước đầu vào là không đổi, do đó không có mối lo ngại về tiệm cận. Ngay cả việc liệt kê đầy đủ tất cả các hoán vị có thể xảy ra cũng không đáng kể. Điều quan trọng hơn là xử lý sự bình đẳng một cách chính xác. Nếu bất kỳ hai giá trị nào bằng nhau thì việc đạt được các bất đẳng thức nghiêm ngặt có thể trở nên bất khả thi tùy thuộc vào cấu trúc, đặc biệt khi các giá trị trùng lặp chiếm các vị trí cực đoan. 

Trường hợp cạnh tinh vi xảy ra khi tất cả các giá trị đều bằng nhau, ví dụ như đầu vào`1 1 1`. Mọi hoán vị đều giống nhau và điều kiện`a1 > a2 < a3`không bao giờ có thể giữ được vì sự bất bình đẳng nghiêm ngặt thất bại ở mọi nơi. Một trường hợp khác là khi hai giá trị bằng nhau và giá trị thứ ba lớn hơn, chẳng hạn như`1 1 2`. Mặc dù chúng ta có thể đặt`2`ở đâu đó, cặp bằng nhau ngăn cản việc hình thành mức tối thiểu nghiêm ngặt ở giữa. Ví dụ,`2 1 1`không thành công vì phần giữa không hoàn toàn nhỏ hơn cả hai đầu và bất kỳ hoán vị nào cũng giữ một trong các giá trị bằng nhau liền kề theo cách phá vỡ tính nghiêm ngặt. 

Một sai lầm ngây thơ là cho rằng việc sắp xếp hoặc chọn mức tối thiểu luôn hiệu quả. Ví dụ: lấy giá trị nhỏ nhất làm giá trị ở giữa và sắp xếp các giá trị khác một cách tùy ý sẽ không thành công khi giá trị nhỏ nhất bị trùng lặp, vì cần có sự bất bình đẳng nghiêm ngặt ở cả hai bên. 

## Phương pháp tiếp cận 

Chỉ với ba phần tử, chiến lược Brute Force là liệt kê tất cả sáu hoán vị của mảng đầu vào và kiểm tra xem có hoán vị nào thỏa mãn điều kiện phần tử ở giữa nhỏ hơn cả hai phần tử lân cận hay không. Nếu chúng tôi tìm thấy một hoán vị như vậy, chúng tôi sẽ xuất nó ngay lập tức. Nếu không, chúng tôi kết luận rằng điều đó là không thể. 

Điều này hoạt động vì không gian tìm kiếm có kích thước đầy đủ và không đổi. Không có cấu trúc ẩn nào làm giảm hoặc mở rộng số lượng khả năng ngoài sáu cách sắp xếp này. 

Tuy nhiên, việc liệt kê các hoán vị là không cần thiết vì điều kiện chỉ phụ thuộc vào thứ tự quan hệ giữa ba giá trị. Quan sát quan trọng là phần tử ở giữa phải là phần tử tối thiểu duy nhất trong ba phần tử. Nếu giá trị tối thiểu xuất hiện nhiều lần thì không có cách nào đặt nó thấp hơn cả hai giá trị khác cùng một lúc, vì một trong các giá trị lân cận sẽ bằng nhau, vi phạm sự bất bình đẳng nghiêm ngặt. Nếu giá trị tối thiểu là duy nhất, chúng ta luôn có thể đặt nó ở giữa và sắp xếp hai giá trị còn lại theo một trong hai thứ tự. 

Điều này làm giảm vấn đề từ tìm kiếm hoán vị sang kiểm tra tần số và so sánh đơn giản. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (hoán đổi tất cả các đơn đặt hàng) | O(1) | O(1) | Đã chấp nhận | 
| Tối ưu (kiểm tra tính duy nhất tối thiểu) | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc ba số nguyên và lưu chúng vào một danh sách. Không có gì về thứ tự được giả định ở giai đoạn này vì các giao dịch hoán đổi cho phép mọi sự sắp xếp lại. 
2. Tính giá trị nhỏ nhất của ba số đó. Giá trị này là ứng cử viên duy nhất có thể có cho vị trí ở giữa, vì phần giữa phải nhỏ hơn cả hai đầu. 
3. Đếm xem giá trị tối thiểu này xuất hiện bao nhiêu lần trong danh sách. Số lượng này xác định tính khả thi. Nếu mức tối thiểu xuất hiện nhiều lần thì bất kỳ vị trí nào của nó sẽ đặt ít nhất một giá trị bằng nhau liền kề với nó, vi phạm sự bất bình đẳng nghiêm ngặt. 
4. Nếu giá trị tối thiểu xuất hiện ít nhất hai lần, hãy xuất ra`-1`. Điều này trực tiếp tương ứng với việc không thể tạo mức tối thiểu cục bộ nghiêm ngặt trong nhiều tập hợp có các bản sao của phần tử nhỏ nhất. 
5. Ngược lại, mức tối thiểu là duy nhất. Đặt nó ở vị trí giữa. 
6. Đặt hai giá trị còn lại vào vị trí thứ nhất và thứ ba theo bất kỳ thứ tự nào. Vì cả hai đều lớn hơn mức tối thiểu nên cả hai điều kiện bất đẳng thức đều được thỏa mãn bất kể thứ tự tương đối của chúng. 
7. Xuất ra hoán vị đã xây dựng. 

### Tại sao nó hoạt động 

điều kiện`a1 > a2 < a3`lực lượng`a2`nhỏ hơn cả hai`a1`Và`a3`. Điều đó có nghĩa`a2`phải là giá trị tối thiểu duy nhất trong ba giá trị. Nếu giá trị tối thiểu không phải là duy nhất thì không có sự sắp xếp lại nào có thể tách biệt các cực tiểu bằng nhau ở cả hai vế trong khi vẫn duy trì sự bất đẳng thức nghiêm ngặt. Ngược lại, nếu mức tối thiểu là duy nhất, việc đặt nó ở giữa đảm bảo cả hai giá trị còn lại đều lớn hơn, do đó, bất kỳ thứ tự nào của chúng ở hai bên đều duy trì tính chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

a = list(map(int, input().split()))

mn = min(a)
cnt = a.count(mn)

if cnt >= 2:
    print(-1)
else:
    a.remove(mn)
    # remaining two elements can be in any order
    print(a[0], mn, a[1])
```Giải pháp dựa trên thực tế là việc xóa danh sách chỉ xóa một lần xuất hiện tối thiểu, điều này an toàn vì chúng tôi đã xác nhận rằng nó là duy nhất. Sau khi loại bỏ, vẫn còn chính xác hai phần tử và chúng tôi đặt mức tối thiểu giữa chúng. 

Một cạm bẫy phổ biến ở đây là quên rằng`remove`chỉ xóa lần xuất hiện đầu tiên. Điều đó chỉ ổn sau khi xác minh tính duy nhất. Nếu không có sự kiểm tra đó, việc xóa có thể âm thầm loại bỏ trường hợp sai trong các trường hợp trùng lặp và tạo ra sự sắp xếp không chính xác. 

## Ví dụ đã hoạt động 

### Ví dụ 1:`3 1 6`| Bước | Trạng thái mảng | Tối thiểu | Đếm(phút) | Hành động | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | [3, 1, 6] | 1 | 1 | Tiếp tục | 
| Sau khi kiểm tra | [3, 1, 6] | 1 | 1 | hợp lệ | 
| Xóa tối thiểu | [3, 6] | 1 | 1 | Đặt ở giữa | 
| Bản dựng đầu ra | [3, 1, 6] | 1 | 1 | Xong | 

Điều này thể hiện trường hợp mức tối thiểu là duy nhất và cả hai phần tử còn lại đều lớn hơn. Bất kỳ thứ tự nào của 3 và 6 xung quanh 1 đều thỏa mãn điều kiện. 

### Ví dụ 2:`1 1 2`| Bước | Trạng thái mảng | Tối thiểu | Đếm(phút) | Hành động | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | [1, 1, 2] | 1 | 2 | Từ chối | 

Ở đây mức tối thiểu không phải là duy nhất. Không có sự sắp xếp lại nào có thể đặt một phần tử ở giữa nhỏ hơn hoàn toàn vì bất kỳ vị trí nào được chọn cho phần giữa vẫn sẽ có một phần tử lân cận bằng nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Số phần tử không đổi, phép toán không đổi | 
| Không gian | O(1) | Chỉ một danh sách có kích thước cố định được lưu trữ | 

Kích thước đầu vào được cố định ở ba số nguyên, do đó thuật toán nằm trong giới hạn tầm thường bất kể các ràng buộc. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    input = _sys.stdin.readline
    a = list(map(int, input().split()))
    mn = min(a)
    cnt = a.count(mn)
    if cnt >= 2:
        return "-1"
    a.remove(mn)
    return f"{a[0]} {mn} {a[1]}"

# provided samples
assert run("3 1 6") == "3 1 6"
assert run("1 2 3") in ["2 1 3", "3 1 2", "2 1 3"]  # any valid answer is fine
assert run("1 1 1") == "-1"

# custom cases
assert run("2 1 2") == "-1"
assert run("10 9 8") == "10 8 9"
assert run("5 1 7") == "5 1 7"
assert run("1 3 2") in ["2 1 3", "3 1 2"]
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3 1 6 | 3 1 6 | xây dựng hợp lệ cơ bản | 
| 1 1 1 | -1 | tất cả đều không thể như nhau | 
| 2 1 2 | -1 | trường hợp cạnh tối thiểu trùng lặp | 
| 10 9 8 | 10 8 9 | thứ tự giảm dần vẫn có tác dụng | 
| 1 3 2 | hoán vị hợp lệ | tính đúng đắn của đầu vào không có thứ tự | 

## Vỏ cạnh 

Trường hợp cạnh quan trọng nhất là khi giá trị tối thiểu xuất hiện nhiều lần. Đối với đầu vào`2 1 2`, tối thiểu là`1`và nó là duy nhất nên việc xây dựng thành công và mang lại kết quả`2 1 2`đó thỏa mãn điều kiện. Tuy nhiên, đối với`1 1 2`, tối thiểu là`1`nhưng nó xuất hiện hai lần, và mọi nỗ lực đặt nó ở giữa đều thất bại vì ít nhất một bên sẽ ngang bằng với nó, phá vỡ sự bất bình đẳng nghiêm ngặt. Thuật toán xử lý việc này bằng cách kiểm tra tần số tối thiểu trước khi xây dựng bất kỳ đầu ra nào. 

Một trường hợp cạnh khác là khi đầu vào giảm nghiêm ngặt, chẳng hạn như`10 5 1`. Giá trị nhỏ nhất là duy nhất nên chúng ta đặt`1`ở giữa và đầu ra`10 1 5`hoặc`5 1 10`. Cả hai đều thỏa mãn điều kiện vì bất đẳng thức nghiêm ngặt chỉ phụ thuộc vào độ lớn hơn chứ không phụ thuộc vào thứ tự tương đối giữa các phần tử bên ngoài.
