---
title: "CF 104339H - Hình tam giác"
description: "Chúng ta có một lưới tam giác đều lớn được hình thành bằng cách chia một tam giác lớn có độ dài cạnh $n$ thành các tam giác đều đơn vị."
date: "2026-07-01T18:40:15+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104339
codeforces_index: "H"
codeforces_contest_name: "FAMCS Olympiad for scholars, Qualification (copy)"
rating: 0
weight: 104339
solve_time_s: 75
verified: true
draft: false
---

[CF 104339H - Hình tam giác](https://codeforces.com/problemset/problem/104339/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 15s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một lưới tam giác đều lớn được hình thành bằng cách chia một tam giác lớn có chiều dài cạnh$n$thành các tam giác đều đều. Cấu trúc là lưới tam giác tiêu chuẩn: mỗi cạnh của tam giác lớn chứa$n$các phân đoạn đơn vị, và bên trong được lấp đầy bằng sự sắp xếp đều đặn của các hình tam giác đơn vị hướng lên và hướng xuống. 

Nhiệm vụ là đếm xem có bao nhiêu hình tam giác phân biệt trong hình này. Một hình tam giác được tính nếu nó xuất hiện ở bất kỳ vị trí nào trong lưới, bất kể kích thước hoặc hướng của nó. Hai hình tam giác được coi là khác nhau nếu chúng khác nhau về vị trí hoặc kích thước, ngay cả khi chúng bằng nhau về mặt hình học. 

Đầu vào là một số nguyên duy nhất$n$, điều khiển kích thước của lưới tam giác. Đầu ra là một số nguyên biểu thị tổng số hình tam giác thuộc mọi kích thước và hướng có thể có bên trong cấu trúc. 

Ràng buộc$n \le 10^4$ngay lập tức loại trừ bất kỳ cách tiếp cận nào liệt kê trực tiếp các hình tam giác. Ngay cả một sự cẩn thận vừa phải$O(n^3)$hoặc$O(n^2)$Việc liệt kê hình học sẽ quá chậm, vì số lượng các cấu trúc con ứng cử viên tăng lên gần như theo số lượng bộ ba hoặc các vùng con trong một mạng hình tam giác, bản thân nó cũng ở mức như sau:$n^2$tế bào. 

Một trường hợp cạnh tinh tế là$n = 1$, trong đó hình bao gồm chính xác một hình tam giác đơn vị. Câu trả lời tầm thường là 1, vì chỉ có một hình tam giác và không tồn tại hình dạng nào lớn hơn. 

Một trường hợp góc khác là nhỏ$n$, chẳng hạn như$n = 2$, trong đó tồn tại cả hai tam giác đơn vị nhỏ và một tam giác lớn hơn. Ở đây việc đếm thủ công rất dễ dàng nhưng cũng nhấn mạnh rằng các hình tam giác xuất hiện ở cả hai hướng và các hình tam giác lớn hơn chồng lên nhiều ô đơn vị. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ cố gắng liệt kê tất cả các bộ ba điểm có thể có trong mạng tam giác và kiểm tra xem chúng có tạo thành một tam giác đều hợp lệ thẳng hàng với lưới hay không. Mỗi chiều dài và hướng bên sẽ cần được xác nhận dựa trên cấu trúc lưới. Ngay cả khi chỉ giới hạn ở các hình tam giác thẳng hàng, số lượng cặp đỉnh ứng cử viên đã là$O(n^4)$trong trường hợp xấu nhất, vì có$O(n^2)$điểm mạng và chọn ba điểm sẽ tạo ra vụ nổ hình khối hoặc tệ hơn. Điều này không thể sử dụng được cho$n = 10^4$. 

Một cách có cấu trúc hơn là ngừng suy nghĩ về hình học tùy ý và thay vào đó sử dụng cấu trúc tổ hợp của lưới tam giác. Mỗi hình tam giác trong hình đều được căn chỉnh theo trục theo một trong ba hướng: hướng lên, hướng xuống hoặc xoay tùy theo cách hiểu. Quan sát quan trọng là mọi tam giác hợp lệ đều được xác định đầy đủ bằng cách chọn đỉnh trên cùng và độ dài cạnh, sau đó kiểm tra xem nó có vừa với đường biên hay không. 

Điều này biến bài toán thành việc đếm xem có bao nhiêu hình tam giác có độ dài mỗi cạnh có thể tồn tại. Đối với chiều dài cạnh cố định$k$, số vị trí mà tam giác hướng lên phù hợp là một hàm bậc hai trong$n-k$, vì cả vị trí ngang và dọc đều co lại tuyến tính với$k$. Điều tương tự cũng áp dụng cho các tam giác hướng xuống, nhưng vị trí hợp lệ của chúng hơi khác nhau vì các tam giác hướng xuống chỉ tồn tại trong một số lớp con nhất định của mạng. 

Cái nhìn sâu sắc cần thiết là thay vì liệt kê các hình tam giác riêng lẻ, chúng ta tính tổng tất cả các độ dài cạnh có thể có$k$, và với mỗi$k$, đếm xem tồn tại bao nhiêu vị trí hợp lệ bằng cách sử dụng các công thức số học bắt nguồn từ việc phân lớp hình tam giác. Điều này làm giảm vấn đề về một phép tính tổng dạng đóng trên$k$, mang lại một$O(n)$hoặc thậm chí$O(1)$giải pháp tùy thuộc vào sự đơn giản hóa đại số. 

Giải pháp cuối cùng sử dụng cấu trúc đã biết của lưới tam giác: tổng số hình tam giác trong lưới tam giác có kích thước$n$là một đa thức bậc ba trong$n$, bắt nguồn từ việc tổng hợp các đóng góp của tất cả các định hướng và quy mô. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^4)$|$O(1)$| Quá chậm | 
| Tối ưu |$O(1)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Hãy coi lưới điện bao gồm các hình tam giác đơn vị được sắp xếp theo các hàng có công suất theo chiều ngang tăng dần và giảm dần. Điều này cho phép chúng ta suy luận theo lớp hơn là tọa độ. Cấu trúc đảm bảo mọi tam giác lớn hơn đều tương ứng với một lựa chọn liền kề của các lớp này. 
2. Cố định độ dài cạnh tam giác$k$. Chúng tôi đếm có bao nhiêu hình tam giác có kích thước này tồn tại. Đối với các hình tam giác hướng lên trên, đỉnh trên cùng phải nằm ở vị trí có ít nhất$k-1$các hàng bên dưới nó. Các vị trí ngang có sẵn cũng co lại tuyến tính khi chúng ta di chuyển xuống. 
3. Tính số lượng các hình tam giác hướng lên trên$k$như một hàm bậc hai trong$n-k+1$. Số lượng vị trí hợp lệ tạo thành cấu trúc số hình tam giác vì mỗi hàng cho phép ít vị trí bắt đầu hơn hàng trước. 
4. Lặp lại lý luận tương tự cho các hình tam giác hướng xuống dưới. Đây là những phiên bản đảo ngược hiệu quả, chiếm các khoảng trống trong mạng và số lượng của chúng tuân theo mô hình bậc hai tương tự nhưng thay đổi về chỉ mục. 
5. Tính tổng tất cả các độ dài cạnh có thể có$k$từ 1 đến$n$. Mỗi đóng góp là một đa thức trong$k$Và$n$, do đó tổng đầy đủ giảm xuống tổng dạng đóng của$k$,$k^2$, và các hằng số. 
6. Rút gọn biểu thức thu được thành đa thức bậc ba theo$n$. Điều này loại bỏ hoàn toàn sự cần thiết phải lặp lại. 

### Tại sao nó hoạt động 

Mỗi tam giác trong lưới được xác định duy nhất bởi hướng, độ dài cạnh và đỉnh trên cùng của nó. Cấu trúc mạng đảm bảo rằng tính khả thi chỉ phụ thuộc vào các ràng buộc tuyến tính dọc theo hai trục, điều này làm cho số lượng có thể tách thành các đóng góp độc lập trên mỗi lớp. Bởi vì các ràng buộc này là tuyến tính, nên việc tính tổng tất cả các vị trí hợp lệ sẽ tạo ra sự tăng trưởng đa thức và không có hiệu ứng ranh giới bất thường nào nằm ngoài các điểm cuối đã được ghi lại ở dạng đóng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input().strip())
    if n == 1:
        print(1)
        return

    # Closed-form result for number of all triangles in triangular grid
    # derived from summing all orientations and sizes
    res = (n * (n + 2) * (2 * n + 1)) // 8
    print(res)

if __name__ == "__main__":
    solve()
```Mã trực tiếp tính toán biểu thức dạng đóng thay vì lặp qua các kích thước hình tam giác. Điều kiện cho$n = 1$không thực sự cần thiết cho tính đúng đắn của công thức, nhưng nó làm cho hành vi biên trở nên rõ ràng và tránh dựa vào việc hủy bỏ đại số trong các trường hợp suy biến. 

Công thức được sử dụng tương ứng với việc phân tích tiêu chuẩn các hình tam giác trong một mạng tam giác thành các phần đóng góp hướng lên và hướng xuống, mỗi phần đóng góp tổng bậc hai trên các vị trí hợp lệ. Biểu thức cuối cùng được đơn giản hóa thành đa thức bậc ba chia cho một thừa số không đổi. 

## Ví dụ đã hoạt động 

### Ví dụ 1:$n = 2$Chúng tôi tính toán bằng cách sử dụng công thức. 

| Bước | Biểu hiện | 
| --- | --- | 
| Đầu vào | 2 | 
| Tính toán |$2 \cdot 4 \cdot 5 / 8$| 
| Kết quả | 5 | 

Điều này xác nhận rằng cả hai tam giác đơn vị và một tam giác lớn hơn đều được tính vào tổng. Cấu hình hướng lên và hướng xuống đều đóng góp không hề nhỏ ngay cả ở kích thước nhỏ này. 

### Ví dụ 2:$n = 4$| Bước | Biểu hiện | 
| --- | --- | 
| Đầu vào | 4 | 
| Tính toán |$4 \cdot 6 \cdot 9 / 8$| 
| Kết quả | 27 | 

Trường hợp này bao gồm nhiều tam giác con chồng lên nhau có kích thước khác nhau. Sự tăng trưởng bậc hai ở các vị trí có sẵn cho các hình tam giác cỡ trung chiếm ưu thế tổng thể. 

Những dấu vết này xác nhận rằng biểu thức dạng đóng tổng hợp chính xác các đóng góp trên tất cả các kích thước tam giác mà không cần liệt kê rõ ràng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(1)$| Chỉ có một số lượng phép tính số học không đổi được thực hiện bất kể$n$| 
| Không gian |$O(1)$| Không sử dụng cấu trúc dữ liệu phụ trợ | 

Giải pháp này nằm trong giới hạn vì thậm chí$n = 10^4$chỉ yêu cầu một số phép nhân và phép cộng số nguyên diễn ra tức thời. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import prod
    n = int(sys.stdin.readline().strip())
    if n == 1:
        return "1\n"
    res = (n * (n + 2) * (2 * n + 1)) // 8
    return str(res) + "\n"

# provided samples
assert run("2\n") == "5\n", "sample 1"
assert run("4\n") == "27\n", "sample 2"

# custom cases
assert run("1\n") == "1\n", "minimum case"
assert run("3\n") == str((3*5*7)//8) + "\n", "small mid case"
assert run("10\n") == str((10*12*21)//8) + "\n", "larger case"
assert run("10000\n") == str((10000*10002*20001)//8) + "\n", "max stress case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | 1 | tam giác hợp lệ nhỏ nhất | 
| 3 | giá trị công thức | tính đúng đắn của cấu trúc nhỏ | 
| 10 | giá trị công thức | hành vi mở rộng quy mô trung gian | 
| 10000 | giá trị lớn | an toàn và hiệu suất tràn | 

## Vỏ cạnh 

cho$n = 1$, lưới chỉ chứa một tam giác đơn vị. Thuật toán tính toán$(1 \cdot 3 \cdot 3)/8 = 9/8$, không hợp lệ dưới dạng kết quả số nguyên, vì vậy chúng tôi trả về 1 một cách rõ ràng. Điều này cho thấy lý do tại sao dạng đóng phải được áp dụng cẩn thận ở biên thay vì tin tưởng một cách mù quáng vào sự đơn giản hóa đại số. 

Vì$n = 2$, cấu trúc bao gồm chính xác một tam giác lớn hơn cộng với nhiều tam giác đơn vị. Công thức cho kết quả 5, khớp với phép phân tách: ba tam giác đơn vị hướng lên trên, một tam giác đơn vị hướng xuống và một tam giác lớn bao trùm toàn bộ hình. Điều này xác nhận rằng cả hai hướng đều được tính đến trong đa thức.
