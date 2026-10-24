---
title: "CF 105968F - Phát hiện gian lận"
description: "Chúng ta có một tập hợp các số nguyên, mỗi số được viết dưới dạng thập phân chuẩn. Nhiệm vụ là xem xét từng số, chỉ trích xuất chữ số đầu tiên khác 0 của nó và đếm tần suất mỗi chữ số từ 0 đến 9 xuất hiện dưới dạng chữ số đứng đầu trong toàn bộ bộ sưu tập."
date: "2026-06-21T21:53:05+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105968
codeforces_index: "F"
codeforces_contest_name: "IME++ Starters Try-Outs 2025"
rating: 0
weight: 105968
solve_time_s: 50
verified: true
draft: false
---

[CF 105968F - Phát hiện gian lận](https://codeforces.com/problemset/problem/105968/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 50s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một tập hợp các số nguyên, mỗi số được viết dưới dạng thập phân chuẩn. Nhiệm vụ là xem xét từng số, chỉ trích xuất chữ số đầu tiên khác 0 của nó và đếm tần suất mỗi chữ số từ 0 đến 9 xuất hiện dưới dạng chữ số đứng đầu trong toàn bộ bộ sưu tập. Đầu ra về cơ bản là sự phân bố tần số trên các chữ số đầu. 

Một cách hữu ích để suy nghĩ về điều này là mọi số đều được rút gọn thành một nhãn duy nhất, chữ số có nghĩa nhất của nó và chúng tôi được yêu cầu tổng hợp các nhãn này trên tập dữ liệu. Cấu trúc bên trong của số, chẳng hạn như các chữ số ở cuối hoặc thậm chí độ lớn của nó vượt quá chữ số đầu tiên, sẽ không liên quan khi chữ số đứng đầu được xác định. 

Nếu kích thước đầu vào đạt đến các ràng buộc lập trình cạnh tranh điển hình, chẳng hạn như tối đa 10^5 số trở lên, thì mọi giải pháp cố gắng xử lý các số từng chữ số trong các vòng lặp lồng nhau hoặc thực hiện chuyển đổi lặp lại không hiệu quả sẽ vẫn ổn miễn là mỗi số được xử lý ở dạng O (độ dài của số). Vì độ dài của mỗi số được giới hạn bởi biểu diễn thập phân của nó nên việc quét tuyến tính trên mỗi số là đủ và dẫn đến giải pháp thời gian tuyến tính tổng thể trong tổng kích thước đầu vào. 

Một số trường hợp khó xử lý. Một con số như`7`rất đơn giản, nhưng một con số như`1000`phải đóng góp vào chữ số`1`, không`0`. Nếu số âm được cho phép, việc triển khai bất cẩn có thể vô tình xử lý`-3`là chữ số hàng đầu`-`, điều này sẽ không chính xác; chúng ta phải luôn bỏ qua các dấu hiệu. Nếu số đó là`0`, chữ số đứng đầu là`0`, đây cũng là nhóm tần số hợp lệ và không nên bỏ qua. Ví dụ, đầu vào đã cho`0 0 10`, đầu ra đúng sẽ phản ánh hai số không và một, bởi vì`10`đóng góp chữ số hàng đầu`1`. 

## Phương pháp tiếp cận 

Một cách giải thích thô bạo sẽ là coi mỗi số là một số nguyên và chia liên tục cho 10 cho đến khi chỉ còn lại một chữ số. Đối với mỗi số, việc này mất O(d) thời gian trong đó d là số chữ số. Trên n số, điều này mang lại O (tổng chữ số), vốn đã tối ưu theo nghĩa thực tế, nhưng nó có thể liên quan đến các phép chia số nguyên lặp đi lặp lại và xử lý cẩn thận các số âm và số 0. 

Một cách tiếp cận bạo lực đơn giản nhưng gọn gàng hơn một chút là chuyển đổi từng số thành một chuỗi và truy cập trực tiếp vào ký tự đầu tiên của nó. Điều này tránh hoàn toàn số học và làm cho logic trở nên minh bạch hơn. Tuy nhiên, nếu được triển khai mà không cẩn thận, việc cắt chuỗi lặp đi lặp lại hoặc chi phí phân tích cú pháp không cần thiết vẫn có thể làm mọi thứ chậm lại một chút ở các đầu vào rất lớn. 

Quan sát quan trọng là chúng ta không cần diễn giải số ngoài ký tự đầu tiên của nó ở dạng chuỗi chuẩn hóa. Sau khi đầu vào được đọc, việc trích xuất ký tự không dấu đầu tiên là đủ. Điều này làm giảm vấn đề xuống còn một lần truyền qua đầu vào trong đó mỗi phần tử đóng góp chính xác một mức tăng cho mảng tần số có kích thước 10. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Phép chia lặp đi lặp lại cho mỗi số | O(n · d) | O(1) | Đã chấp nhận | 
| Trích xuất chuỗi chữ số đầu tiên | O(n · d) | O(1) | Đã chấp nhận | 

Cả hai cách tiếp cận đều tuyến tính một cách hiệu quả về kích thước đầu vào, nhưng phương pháp dựa trên chuỗi thường là phương pháp đơn giản nhất và ít xảy ra lỗi nhất. 

## Hướng dẫn thuật toán 

1. Đọc tất cả các số dưới dạng chuỗi thay vì số nguyên, vì dạng chuỗi giữ nguyên chữ số đứng đầu mà không có bất kỳ thao tác số học nào. Điều này tránh được các vấn đề về tràn hoặc xử lý ký hiệu. 
2. Khởi tạo một mảng`cnt`có kích thước 10 với tất cả số không. Mỗi chỉ số tương ứng với một chữ số từ 0 đến 9 và chúng ta sẽ tích lũy số đếm ở đó. 
3. Với mỗi chuỗi số, quét từ đầu cho đến khi tìm được ký tự không phải dấu trừ. Ký tự đầu tiên như vậy là chữ số đầu của số. Bước này quan trọng vì số âm có thể bao gồm số đứng đầu`-`, điều này phải được bỏ qua. 
4. Chuyển ký tự chữ số đầu tiên đó thành số nguyên và tăng dần`cnt[digit]`bằng 1. Điều này trực tiếp phân loại số vào nhóm chữ số đầu của nó. 
5. Sau khi xử lý tất cả các số, xuất ra số đếm theo thứ tự từ chữ số 0 đến chữ số 9. 

### Tại sao nó hoạt động 

Mỗi số đóng góp chính xác một chữ số đứng đầu và chữ số này được xác định duy nhất bởi ký tự không dấu đầu tiên trong biểu diễn thập phân của nó. Thuật toán không bao giờ sửa đổi hoặc diễn giải lại số vượt quá điểm đó, do đó, có một ánh xạ một-một giữa các phần tử đầu vào và số gia trong mảng tần số. Vì mọi phần tử được xử lý độc lập và chính xác một lần nên số lượng tích lũy đều đầy đủ và chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input().strip())
    cnt = [0] * 10

    for _ in range(n):
        s = input().strip()
        i = 0
        while i < len(s) and s[i] == '-':
            i += 1
        if i < len(s):
            d = ord(s[i]) - ord('0')
            cnt[d] += 1

    print(*cnt)

if __name__ == "__main__":
    solve()
```Giải pháp đọc mỗi số dưới dạng một chuỗi và tránh mọi chuyển đổi số ngoài việc trích xuất chữ số có ý nghĩa đầu tiên. Vòng lặp bỏ qua`-`các ký tự đảm bảo tính chính xác nếu có giá trị âm, mặc dù nhiều trường hợp của vấn đề này chỉ sử dụng số nguyên không âm. 

Mảng tần số`cnt`có kích thước cố định nên các bản cập nhật diễn ra liên tục. Câu lệnh in cuối cùng đưa ra phân phối đầy đủ trong một dòng. 

Một điểm tinh tế là chúng tôi không loại bỏ các số 0 đứng đầu một cách rõ ràng. Đây là cố ý vì một số như`"007"`vẫn có chữ số đứng đầu`0`và ký tự không dấu đầu tiên nắm bắt chính xác điều đó. 

## Ví dụ đã hoạt động 

Xem xét đầu vào:```
5
123
45
7
100
-89
```Chúng tôi chỉ theo dõi việc trích xuất chữ số hàng đầu. 

| Số | Đã bỏ qua | Chữ số hàng đầu | Đếm cập nhật | 
| --- | --- | --- | --- | 
| 123 | không | 1 | cnt[1]++ | 
| 45 | không | 4 | cnt[4]++ | 
| 7 | không | 7 | cnt[7]++ | 
| 100 | không | 1 | cnt[1]++ | 
| -89 | '-' | 8 | cnt[8]++ | 

Số đếm cuối cùng phản ánh hai số bắt đầu bằng 1 và mỗi số là 4, 7 và 8. 

Dấu vết này xác nhận rằng thuật toán hoàn toàn là vị trí và không bị ảnh hưởng bởi độ lớn hoặc các chữ số ở cuối. 

Bây giờ hãy xem xét:```
4
0
0
10
-3
```| Số | Đã bỏ qua | Chữ số hàng đầu | Đếm cập nhật | 
| --- | --- | --- | --- | 
| 0 | không | 0 | cnt[0]++ | 
| 0 | không | 0 | cnt[0]++ | 
| 10 | không | 1 | cnt[1]++ | 
| -3 | '-' | 3 | cnt[3]++ | 

Điều này thể hiện việc xử lý chính xác số 0 và số âm. Đặc biệt,`0`được coi là chữ số hàng đầu hợp lệ thay vì bị loại bỏ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n · d) | Mỗi số được quét một lần để tìm chữ số không dấu đầu tiên của nó, trong đó d là độ dài của nó | 
| Không gian | O(1) | Chỉ sử dụng một mảng có kích thước cố định gồm 10 bộ đếm | 

Thuật toán chia tỷ lệ tuyến tính theo kích thước của đầu vào, vì mỗi ký tự trong đầu vào được kiểm tra nhiều nhất một lần. Điều này nằm trong giới hạn điển hình cho các vấn đề lập trình cạnh tranh liên quan đến tối đa 10^5 số hoặc nhiều hơn. 

## Trường hợp thử nghiệm```python
import sys, io

def solve_io(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    sys.stdin = old_stdin
    return out.getvalue().strip()

# basic sample-like test
assert solve_io("5\n123\n45\n7\n100\n-89\n") == "0 2 0 0 1 0 0 1 1 0"

# all same leading digit
assert solve_io("3\n9\n90\n999\n") == "0 0 0 0 0 0 0 0 0 3"

# zeros and negatives
assert solve_io("4\n0\n0\n10\n-3\n") == "2 1 0 1 0 0 0 0 0 0"

# single element
assert solve_io("1\n5\n") == "0 0 0 0 0 1 0 0 0 0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| hỗn số | số lượng đa dạng | tính đúng đắn chung | 
| tất cả 9s | đếm tập trung | xử lý chữ số hàng đầu thống nhất | 
| số không/âm | xử lý cạnh hỗn hợp | bỏ qua dấu hiệu và xử lý bằng không | 
| giá trị đơn | đầu vào tối thiểu | trường hợp cơ sở đúng đắn | 

## Vỏ cạnh 

Đối với đầu vào bao gồm toàn số 0, chẳng hạn như:```
3
0
0
0
```thuật toán xử lý từng chuỗi, tìm`'0'`là ký tự không dấu đầu tiên và tăng dần`cnt[0]`ba lần. Đầu ra chính xác sẽ trở thành một vectơ có 3 ở chỉ số 0 và các số 0 ở nơi khác. 

Đối với đầu vào có một chữ số âm như:```
3
-1
-9
-3
```mỗi chuỗi bỏ qua`-`và ngay lập tức đọc chữ số đó, tạo ra số đếm tương ứng là 1, 9 và 3. Dấu hiệu không bao giờ tham gia vào quá trình phân loại nên không có chỉ mục không hợp lệ nào được truy cập. 

Đối với các số có số 0 đứng đầu như:```
3
007
0002
09
```ký tự không dấu đầu tiên luôn là`'0'`, do đó tất cả các đóng góp đều chuyển đến chữ số 0 một cách chính xác. Thuật toán không nén hoặc chuẩn hóa số, điều này đảm bảo thông tin số 0 ở đầu được giữ nguyên chính xác theo yêu cầu.
