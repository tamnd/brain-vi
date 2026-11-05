---
title: "CF 1201B - Mảng không"
description: "Chúng ta được cung cấp một danh sách các số và chúng ta liên tục thực hiện một thao tác chọn hai vị trí khác nhau và giảm cả hai giá trị đi một. Mục tiêu là để quyết định liệu cuối cùng chúng ta có thể hạ mọi giá trị xuống chính xác bằng 0 hay không bằng cách sử dụng một số chuỗi thao tác như vậy."
date: "2026-06-15T17:33:47+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1201
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 577 (Div. 2)"
rating: 1500
weight: 1201
solve_time_s: 305
verified: true
draft: false
---

[CF 1201B - Mảng không](https://codeforces.com/problemset/problem/1201/B) 

**Đánh giá:** 1500 
**Tags:** tham lam, toán học 
**Thời gian giải:** 5 phút 5s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một danh sách các số và chúng ta liên tục thực hiện một thao tác chọn hai vị trí khác nhau và giảm cả hai giá trị đi một. Mục tiêu là để quyết định liệu cuối cùng chúng ta có thể hạ mọi giá trị xuống chính xác bằng 0 hay không bằng cách sử dụng một số chuỗi thao tác như vậy. 

Mỗi hoạt động tiêu thụ chính xác một đơn vị từ hai phần tử riêng biệt. Nếu bạn nghĩ về mặt “đơn vị công việc”, mọi thao tác sẽ loại bỏ tổng cộng hai đơn vị, nhưng chúng phải đến từ hai nhóm khác nhau cùng một lúc. Câu hỏi đặt ra là liệu việc phân phối các đơn vị ban đầu trên toàn mảng có thể được ghép nối hoàn toàn như thế này cho đến khi không còn gì hay không. 

Sự ràng buộc về`n`lên đến$10^5$với các giá trị lên tới$10^9$ngay lập tức loại trừ mọi hoạt động mô phỏng. Ngay cả khi chúng ta cố gắng mô phỏng sự giảm dần một cách tham lam, tổng số thao tác có thể ở mức$10^9$, vượt xa giới hạn khả thi. Thay vào đó, giải pháp phải dựa vào điều kiện cấu trúc có thể được kiểm tra theo thời gian tuyến tính. 

Một vài trường hợp thất bại tinh tế sẽ xuất hiện nếu chúng ta cố gắng suy luận một cách ngây thơ. Ví dụ, người ta có thể nghĩ rằng miễn là tổng là số chẵn thì câu trả lời là “CÓ”. Điều này không thành công: hãy xem xét`[1, 1, 1]`, trong đó tổng là 3 (đã là số lẻ nên bị từ chối), nhưng cũng`[2, 2, 1, 1]`có tổng 6 và có vẻ đầy hứa hẹn. Tuy nhiên, các ràng buộc ghép đôi vẫn có thể khiến điều đó không thể thực hiện được nếu một yếu tố chiếm ưu thế quá nhiều. 

Một ý tưởng ngây thơ khác là ghép nối các phần tử lớn nhất một cách tham lam cho đến khi cạn kiệt. Điều này có thể thất bại trong những trường hợp như`[3, 3, 1, 1]`, nơi việc ghép đôi tham lam hoạt động, nhưng trong`[4, 1, 1, 1]`, việc ghép đôi tham lam nhanh chóng bị mắc kẹt mặc dù bản thân cấu trúc đã đảm bảo là không thể thực hiện được. 

Vì vậy, khó khăn thực sự không phải là mô phỏng mà là nhận biết khi nào tồn tại cấu trúc ghép nối hợp lệ. 

## Phương pháp tiếp cận 

Mỗi thao tác sẽ loại bỏ một đơn vị khỏi hai chỉ số riêng biệt, do đó quá trình này có thể được hiểu là ghép các đóng góp của đơn vị riêng lẻ trên các chỉ số khác nhau. Hãy tưởng tượng mỗi phần tử mảng là một chồng mã thông báo đơn vị. Chúng tôi cần khớp mọi mã thông báo với mã thông báo từ một ngăn xếp khác nhau. 

Nếu chúng ta mở rộng mảng thành nhiều tập hợp các mã thông báo đơn vị, thì nhiệm vụ sẽ trở thành: liệu chúng ta có thể ghép tất cả các mã thông báo sao cho không có cặp nào xuất phát từ cùng một chỉ mục không? Điều này ngay lập tức gợi ý hai điều kiện cần thiết. 

Đầu tiên, tổng số mã thông báo phải là số chẵn, vì mỗi thao tác sẽ loại bỏ chính xác hai đơn vị. Nếu tổng là số lẻ thì ít nhất một đơn vị sẽ không được ghép đôi. 

Thứ hai, không có phần tử đơn lẻ nào có thể quá lớn so với phần còn lại. Nếu một giá trị vượt quá tổng của tất cả các giá trị khác thì không thể tiếp tục ghép các đơn vị của nó với các phần tử riêng biệt vì cuối cùng chúng tôi sẽ hết mã thông báo bên ngoài để khớp với nó. Về mặt hình thức, nếu`max(a)`lớn hơn`sum(a) - max(a)`, thì đống lớn nhất không thể được ghép hoàn toàn. 

Hai điều kiện này cũng đủ. Nếu chúng giữ nguyên, chúng ta luôn có thể xây dựng các cặp tham lam mà không bị mắc kẹt, bởi vì các cọc lớn luôn được hỗ trợ bởi các cọc nhỏ hơn đủ để hấp thụ sự suy giảm của chúng. 

Ý tưởng brute-force sẽ mô phỏng các hoạt động, luôn chọn hai phần tử khác 0 và giảm dần chúng cho đến khi không còn bước di chuyển nào hoặc đạt đến tất cả các số 0. Điều này hoạt động về mặt khái niệm nhưng đòi hỏi khả năng$O(\sum a_i)$hoạt động không thể thực hiện được khi giá trị lớn. 

Điểm mấu chốt là chúng ta không cần mô phỏng việc ghép nối; chúng ta chỉ cần xác minh các điều kiện khả thi bắt nguồn từ việc bảo toàn các đơn vị và các ràng buộc ghép nối. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(tổng a_i) | O(1) | Quá chậm | 
| Kiểm tra toán học | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính tổng của mảng. Điều này thể hiện tổng số lần loại bỏ đơn vị được yêu cầu trong tất cả các hoạt động. Vì mỗi phép toán loại bỏ chính xác 2 đơn vị nên tổng này phải chẵn để bất kỳ giải pháp nào tồn tại. 
2. Nếu tổng số là số lẻ thì kết luận ngay là không thể. Không có chuỗi hoạt động nào có thể loại bỏ một đơn vị còn sót lại. 
3. Xác định phần tử lớn nhất trong mảng. Giá trị này đại diện cho ngăn xếp bị hạn chế nhất, vì mỗi đơn vị trong đó phải được ghép nối với một đơn vị từ chỉ mục khác. 
4. Tính tổng tất cả các phần tử còn lại không bao gồm phần tử lớn nhất. Điều này thể hiện tổng “công suất bên ngoài” có sẵn có thể được ghép nối với đống lớn nhất. 
5. Nếu phần tử lớn nhất lớn hơn tổng còn lại này, trả về “NO”. Điều này cho thấy rằng ngay cả khi chúng tôi sử dụng mọi phần tử khác một cách tối ưu, chúng tôi vẫn sẽ không có đủ đơn vị riêng biệt để khớp với tất cả các đơn vị trong đống lớn nhất. 
6. Nếu không, trả về “CÓ”, vì cả hai ràng buộc về tính chẵn lẻ toàn cầu và tính khả thi cục bộ đều được thỏa mãn. 

### Tại sao nó hoạt động 

Quá trình này có thể được xem như việc xây dựng các cạnh trong một sự so khớp hoàn chỉnh trong đó mỗi chỉ số đóng góp số đỉnh bằng giá trị của nó. Các ràng buộc đảm bảo rằng sự kết hợp hoàn hảo tồn tại mà không cần tự ghép nối. 

Điều kiện chẵn lẻ buộc rằng về nguyên tắc thậm chí có thể thực hiện được một kết hợp hoàn hảo. Điều kiện ưu thế đảm bảo không có tập đỉnh nào quá lớn để có thể khớp với bên ngoài. Cùng với nhau, những điều này đảm bảo rằng quá trình ghép đôi tham lam luôn có thể tiếp tục mà không bị mắc kẹt, bởi vì ở mỗi giai đoạn vẫn còn ít nhất một chỉ mục thay thế để ghép với bất kỳ đơn vị nào được chọn từ đống lớn nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    
    total = sum(a)
    mx = max(a)
    
    if total % 2 != 0:
        print("NO")
        return
    
    if mx > total - mx:
        print("NO")
        return
    
    print("YES")

if __name__ == "__main__":
    solve()
```Mã đầu tiên tính tổng và giá trị tối đa trong một lần chuyển. Kiểm tra tính chẵn lẻ được thực hiện trước bất kỳ điều gì khác vì đây là điều kiện cần thiết nghiêm ngặt. Kiểm tra thứ hai thực thi tính khả thi của việc ghép nối phần tử lớn nhất với tất cả các phần tử khác được kết hợp. Thứ tự chỉ quan trọng đối với hiệu quả thoát sớm chứ không phải tính đúng đắn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
1 1 2 2
```| Bước | Tổng số tiền | Tối đa | Tổng còn lại | Quyết định | 
| --- | --- | --- | --- | --- | 
| 1 | 6 | 2 | 4 | tiếp tục | 
| 2 | thậm chí | - | - | tiếp tục | 
| 3 | tối đa ≤ phần còn lại | - | - | CÓ | 

Điều này xác nhận cả hai điều kiện đều được thỏa mãn, do đó có thể ghép nối đầy đủ. 

### Ví dụ 2 

đầu vào:```
3
3 1 1
```| Bước | Tổng số tiền | Tối đa | Tổng còn lại | Quyết định | 
| --- | --- | --- | --- | --- | 
| 1 | 5 | 3 | 2 | dừng lại | 
| 2 | lẻ | - | - | KHÔNG | 

Tổng là số lẻ nên chúng ta bác bỏ ngay lập tức mà không cần xem xét thêm về cấu trúc. 

Những dấu vết này cho thấy cách thuật toán lọc sớm các trường hợp không thể bằng cách sử dụng các bất biến toàn cục thay vì mô phỏng cục bộ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | một lần để tính tổng và tối đa | 
| Không gian | O(1) | chỉ một số biến tích lũy được sử dụng | 

Các ràng buộc cho phép lên đến$10^5$các phần tử, do đó quét tuyến tính duy nhất là tối ưu và thoải mái trong giới hạn. Không cần cấu trúc trung gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque
    input = sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))
    
    total = sum(a)
    mx = max(a)
    
    if total % 2 != 0:
        return "NO"
    if mx > total - mx:
        return "NO"
    return "YES"

# provided sample
assert run("4\n1 1 2 2\n") == "YES"

# all equal small
assert run("2\n1 1\n") == "YES"

# impossible due to dominance
assert run("3\n4 1 1\n") == "NO"

# odd sum
assert run("3\n1 1 1\n") == "NO"

# large valid
assert run("5\n2 2 2 2 2\n") == "YES"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 1 1 | CÓ | trường hợp hợp lệ tối thiểu | 
| 3 1 1 1 | KHÔNG | từ chối số tiền lẻ | 
| 3 4 1 1 | KHÔNG | hạn chế thống trị | 
| 5 2 2 2 2 2 | CÓ | trường hợp hợp lệ lớn thống nhất | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi một phần tử chính xác bằng tổng của tất cả các phần tử khác. Ví dụ,`[4, 1, 1, 2]`. Tổng số là 8, là số chẵn và lớn nhất là 4 trong khi tổng còn lại cũng là 4. Điều kiện`mx > total - mx`là sai nên thuật toán chấp nhận trường hợp đó. Điều này đúng vì đống lớn nhất có thể được ghép chính xác với tất cả các đơn vị còn lại mà không có thức ăn thừa. 

Một trường hợp tế nhị khác là`[1, 1]`. Tổng là 2, chẵn và tối đa là 1 trong khi tổng còn lại cũng là 1. Thuật toán chấp nhận nó, khớp với một thao tác khả thi duy nhất, trực tiếp giảm cả hai phần tử về 0. 

Một trường hợp như`[3, 3, 3]`bị từ chối vì tổng bằng 9, ngay lập tức không có tính chẵn lẻ. Mặc dù các giá trị được cân bằng, việc ghép nối luôn loại bỏ hai đơn vị, do đó chắc chắn vẫn còn một đơn vị.
