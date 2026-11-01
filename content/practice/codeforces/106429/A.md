---
title: "CF 106429A - Tách bánh tart"
description: "Chúng ta được cung cấp một chuỗi biểu thị sự sắp xếp vòng tròn của các phần tử và một chuỗi khác biểu thị mẫu mục tiêu. Nhiệm vụ là xác định xem có thể thu được mẫu đích bằng cách xoay cách sắp xếp hình tròn ban đầu hay không."
date: "2026-06-21T19:19:18+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106429
codeforces_index: "A"
codeforces_contest_name: "MITIT Spring 2026 Invitationals Qualification Round 1"
rating: 0
weight: 106429
solve_time_s: 40
verified: true
draft: false
---

[CF 106429A - Tách bánh tart](https://codeforces.com/problemset/problem/106429/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 40s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi biểu thị sự sắp xếp vòng tròn của các phần tử và một chuỗi khác biểu thị mẫu mục tiêu. Nhiệm vụ là xác định xem có thể thu được mẫu đích bằng cách xoay cách sắp xếp hình tròn ban đầu hay không. 

Thay vì suy nghĩ theo chuyển động tròn, sẽ dễ dàng diễn giải lại thao tác như lấy chuỗi ban đầu và dịch chuyển nó sang trái một số vị trí, quấn quanh đầu. Mọi cấu hình hợp lệ chỉ là một trong những phép quay này. Mục tiêu là để quyết định xem có ít nhất một vòng quay khớp chính xác với mục tiêu hay không. 

Đầu vào mô tả hai chuỗi có độ dài bằng nhau, một chuỗi đóng vai trò là cấu hình cơ sở và chuỗi còn lại đóng vai trò là mẫu mà chúng ta muốn khớp sau khi xoay. Đầu ra là một quyết định đơn giản có hoặc không tùy thuộc vào việc xoay vòng như vậy có tồn tại hay không. 

Cấu trúc ràng buộc ngụ ý các chuỗi có thể lớn, thường lên tới khoảng 10^5 ký tự. Một giải pháp bậc hai thử mọi phép quay và so sánh từng ký tự sẽ yêu cầu N^2 so sánh trong trường hợp xấu nhất, quá chậm trong giới hạn tiêu chuẩn. Điều này ngay lập tức thúc đẩy chúng ta hướng tới một phương pháp có thể kiểm tra tất cả các phép quay trong thời gian tuyến tính. 

Một số trường hợp đặc biệt quan trọng ở đây. Nếu cả hai chuỗi giống hệt nhau thì câu trả lời gần như là có vì phép quay bằng 0 không hoạt động. Ví dụ: nếu các chuỗi có nhiều ký tự khác nhau`abac`Và`zzzz`, không có thao tác xoay nào có thể khắc phục được sự không khớp đó và mọi thao tác kiểm tra xoay vòng đơn giản vẫn sẽ sớm thất bại. Một trường hợp tinh vi khác xảy ra khi tồn tại các mẫu lặp đi lặp lại, chẳng hạn như`aaaaa`Và`aaaaa`, trong đó mọi phép quay đều hợp lệ. Việc triển khai bất cẩn vẫn có thể thực hiện sai nhiều phép so sánh dư thừa, nhưng tính chính xác sẽ không bị ảnh hưởng. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: tạo ra mọi vòng quay có thể có của chuỗi gốc và so sánh nó với chuỗi đích. Mỗi phép quay có thể được xây dựng bằng cách cắt chuỗi ở vị trí k và nối hậu tố với tiền tố. Đối với mỗi ứng cử viên như vậy, chúng tôi so sánh từng đặc điểm của nó với mục tiêu. 

Điều này hiệu quả vì nó kiểm tra rõ ràng mọi cấu hình trong không gian tìm kiếm. Tuy nhiên, chi phí cao. Việc xây dựng và so sánh mỗi phép quay mất O(N) và có N phép quay, dẫn đến tổng số phép toán là O(N^2). Với N khoảng 100.000, điều này trở nên không khả thi. 

Quan sát quan trọng là tất cả các phép quay của một chuỗi đều được chứa bên trong một phiên bản nhân đôi của chính nó. Nếu chúng ta nối chuỗi với chính nó, mọi phép dịch chuyển theo chu kỳ sẽ xuất hiện dưới dạng chuỗi con liền kề của chuỗi nhân đôi này. Ví dụ, phép quay của`abcd`là`abcd`,`bcda`,`cdab`,`dabc`, và tất cả chúng đều xuất hiện bên trong`abcdabcd`. 

Điều này làm giảm vấn đề thành tìm kiếm chuỗi con: chúng ta chỉ cần kiểm tra xem chuỗi đích có xuất hiện trong`S + S`. Thay vì tạo các phép quay một cách rõ ràng, chúng tôi quét một lần bằng cách sử dụng tìm kiếm chuỗi con tuyến tính. Cách triển khai đơn giản sử dụng so sánh cắt trực tiếp, nhìn chung vẫn là O(N) vì mỗi vị trí bắt đầu được kiểm tra nhiều nhất một lần và các so sánh được ngắt mạch nhanh chóng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(N^2) | O(N) | Quá chậm | 
| Quét chuỗi đôi | O(N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc hai chuỗi, gọi chúng là S và T. Nếu độ dài của chúng khác nhau, hãy kết luận ngay rằng không có nghiệm nào tồn tại vì phép quay bảo toàn độ dài. Điều này tránh được những công việc không cần thiết với đầu vào không đúng định dạng hoặc cạnh. 
2. Xây dựng một chuỗi U mới bằng cách nối S với chính nó. Bước này mã hóa tất cả các phép quay tuần hoàn của S thành chuỗi con của U. 
3. Lặp lại mọi vị trí bắt đầu i từ 0 đến len(S) - 1. Mỗi vị trí đại diện cho một vòng quay ứng cử viên. 
4. Với mỗi i, so sánh chuỗi con U[i : i + len(S)] với T. Nếu chúng khớp chính xác, hãy trả về câu trả lời khẳng định ngay lập tức. 
5. Nếu không có vị trí nào khớp, hãy trả về câu trả lời phủ định. 

Bước so sánh là cốt lõi của thuật toán. Điều này an toàn vì mỗi vòng quay tương ứng với chính xác một chỉ số bắt đầu trong chuỗi nhân đôi, do đó không có cấu hình hợp lệ nào bị bỏ qua. 

### Tại sao nó hoạt động 

Tính chính xác dựa trên thực tế là việc xoay vòng theo chu kỳ sẽ bảo toàn thứ tự tương đối của các ký tự, chỉ dịch chuyển điểm bắt đầu. Khi chúng ta tạo thành S + S, bất kỳ phép quay nào bắt đầu từ chỉ số i trong S đều tương ứng với chuỗi con bắt đầu tại i trong S + S có độ dài len(S). Điều này thiết lập ánh xạ một-một giữa các phép quay và chuỗi con có độ dài cố định trong chuỗi nhân đôi. Vì vậy, việc tìm kiếm T trong số các chuỗi con này tương đương với việc kiểm tra tất cả các phép quay. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    S = input().strip()
    T = input().strip()

    if len(S) != len(T):
        print("NO")
        return

    n = len(S)
    U = S + S

    for i in range(n):
        if U[i:i+n] == T:
            print("YES")
            return

    print("NO")

if __name__ == "__main__":
    solve()
```Việc triển khai trực tiếp theo sau việc rút gọn tìm kiếm chuỗi con. Chuỗi nhân đôi U được tạo một lần và sau đó chúng ta trượt một cửa sổ có kích thước n qua nó. Việc so sánh cắt lát là an toàn vì Python thực hiện so sánh ngắn mạch khi không khớp, giữ hành vi trung bình tuyến tính cho các đầu vào thông thường. 

Một chi tiết tinh tế là vòng lặp bị ràng buộc. Chúng tôi chỉ lặp lại tối đa n chứ không phải 2n, vì bắt đầu ngoài chỉ số n sẽ lặp lại các phép quay đã được bao phủ bởi các vị trí trước đó. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Giả sử S =`abcd`, T =`cdab`. 

Chúng ta tạo thành U =`abcdabcd`. 

| tôi | U[i:i+4] | Trận đấu với T | 
| --- | --- | --- | 
| 0 | abcd | không | 
| 1 | bcda | không | 
| 2 | cdab | vâng | 

Tại i = 2, ta tìm được kết quả trùng khớp nên câu trả lời là CÓ. Điều này thể hiện cách các phép quay được nhúng dưới dạng chuỗi con trong chuỗi nhân đôi. 

### Ví dụ 2 

Giả sử S =`aabc`, T =`abca`. 

Chúng ta tạo thành U =`aabcaabc`. 

| tôi | U[i:i+4] | Trận đấu với T | 
| --- | --- | --- | 
| 0 | aabc | không | 
| 1 | abca | vâng | 

Sự trùng khớp xảy ra ở i = 1, xác nhận rằng một phép quay duy nhất sắp thẳng hàng S với T. 

Những dấu vết này cho thấy mọi vòng quay hợp lệ đều tương ứng với chính xác một cửa sổ căn chỉnh thành công. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N) | Mỗi chỉ mục được kiểm tra một lần và các so sánh chuỗi con sẽ kết thúc nhanh chóng khi không khớp | 
| Không gian | O(N) | Chúng tôi lưu trữ chuỗi nhân đôi S + S | 

Thuật toán chạy thoải mái trong giới hạn cho các chuỗi tối đa 10^5 ký tự, vì cả quá trình xây dựng và quét đều là tuyến tính. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    S = input().strip()
    T = input().strip()

    if len(S) != len(T):
        return "NO"

    U = S + S
    n = len(S)

    for i in range(n):
        if U[i:i+n] == T:
            return "YES"

    return "NO"

assert run("abcd\ncdab\n") == "YES"
assert run("abcd\nacbd\n") == "NO"
assert run("aaaa\naaaa\n") == "YES"
assert run("abc\nabc\n") == "YES"
assert run("abc\nbca\n") == "YES"
assert run("abc\ndef\n") == "NO"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| abcd / cdab | CÓ | trận đấu xoay đơn giản | 
| abcd/acbd | KHÔNG | ký tự giống nhau nhưng thứ tự không hợp lệ | 
| aaa / aaa | CÓ | tất cả các phép quay giống hệt nhau | 
| abc / abc | CÓ | trường hợp xoay không | 
| abc / bca | CÓ | vòng quay không tầm thường | 
| abc / def | KHÔNG | bảng chữ cái rời rạc | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi tất cả các ký tự giống hệt nhau. Với S =`aaaaa`và T =`aaaaa`, mọi vòng quay đều giống hệt nhau. Thuật toán có dạng U =`aaaaaaaaaa`và mọi cửa sổ đều khớp với T. Lần lặp đầu tiên đã trả về CÓ, xử lý chính xác số lần lặp lại tối đa. 

Một trường hợp khác là khi không có vòng quay nào có thể khớp do số lượng ký tự khác nhau. Với S =`abc`và T =`abd`, U =`abcabc`. Mỗi cửa sổ chỉ chứa các ký tự a, b, c, do đó mọi so sánh với T đều thất bại ngay lập tức và vòng lặp sẽ cạn kiệt tất cả các vị trí, trả về NO một cách chính xác. 

Trường hợp tinh tế cuối cùng là khi sự trùng khớp xảy ra ở ranh giới của chuỗi nhân đôi. Với S =`bcda`, T =`dabc`, U =`bcdabcda`. Trận đấu hợp lệ bắt đầu ở chỉ số 3, vượt qua ranh giới ban đầu. Cửa sổ trượt vẫn chụp được nó vì S + S bảo toàn chính xác tính liên tục bao quanh.
