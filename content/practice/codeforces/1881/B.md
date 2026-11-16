---
title: "CF 1881B - Ba ren"
description: "Chúng ta có ba số nguyên dương, mỗi số đại diện cho độ dài của một sợi dây. Trong một lần di chuyển, chúng tôi chọn một luồng duy nhất và chia nó thành hai luồng nhỏ hơn có độ dài nguyên, cả hai đều dương và tổng của chúng bằng độ dài ban đầu."
date: "2026-06-08T22:39:37+07:00"
tags: ["codeforces", "competitive-programming", "math"]
categories: ["algorithms"]
codeforces_contest: 1881
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 903 (Div. 3)"
rating: 900
weight: 1881
solve_time_s: 87
verified: true
draft: false
---

[CF 1881B - Ba luồng nhỏ](https://codeforces.com/problemset/problem/1881/B) 

**Xếp hạng:** 900 
**Thẻ:** toán 
**Thời gian giải:** 1 phút 27s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có ba số nguyên dương, mỗi số đại diện cho độ dài của một sợi dây. Trong một lần di chuyển, chúng tôi chọn một luồng duy nhất và chia nó thành hai luồng nhỏ hơn có độ dài nguyên, cả hai đều dương và tổng của chúng bằng độ dài ban đầu. Tổng cộng chúng tôi được phép chia tối đa ba phần như vậy trên tất cả các chuỗi. Sau khi thực hiện một số chuỗi phân tách, chúng tôi muốn tất cả các chuỗi kết quả có cùng độ dài. 

Đầu ra chính là kiểm tra tính khả thi: đối với mỗi trường hợp thử nghiệm, chúng ta phải quyết định xem liệu có thể kết thúc bằng một tập hợp các phần có độ dài bằng nhau bằng cách sử dụng không quá ba phần tách hay không. 

Các ràng buộc rất lớn về số lượng kiểm tra, lên tới 10^4 trường hợp, trong khi mỗi trường hợp kiểm tra có kích thước không đổi. Điều này ngay lập tức loại trừ bất kỳ cách tiếp cận nào mô phỏng các cấu hình phân tách tùy ý hoặc tìm kiếm trên các trạng thái, vì ngay cả một BFS phân nhánh vừa phải trên các phân vùng cũng sẽ bùng nổ. Mỗi bài kiểm tra phải được trả lời trong thời gian cố định hoặc gần như không đổi. 

Một điểm tinh tế là số lượng mảnh ghép không cố định. Mỗi thao tác đều tăng tổng số luồng con lên một, do đó, sau nhiều nhất ba thao tác, chúng ta có thể có tổng cộng tối đa sáu phần. Giới hạn cứng này về kích thước trạng thái cuối cùng là điều làm cho lý luận tổ hợp trực tiếp trở nên khả thi. 

Các trường hợp đặc biệt có xu hướng phá vỡ lối suy nghĩ ngây thơ bao gồm các tình huống trong đó một giá trị lớn hơn nhiều so với các giá trị khác, chẳng hạn như (1, 1, 1000000000) hoặc các trường hợp trong đó hai giá trị đã bằng nhau nhưng giá trị thứ ba hơi lệch một chút, chẳng hạn như (5, 5, 6). Những chiến lược này thường cám dỗ những chiến lược “cân bằng bằng cách chia lớn nhất” tham lam, nhưng lại bỏ qua giới hạn hoạt động nghiêm ngặt và ràng buộc số nguyên về kích thước phân khúc. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ sẽ là mô phỏng tất cả các cách có thể để thực hiện tối đa ba lần cắt. Mỗi lần cắt chọn một đoạn hiện tại và chia nó thành hai số nguyên dương. Hệ số phân nhánh tăng theo độ dài của các đoạn, vì một đoạn có độ dài L có khả năng phân chia là L−1. Ngay cả chỉ với ba thao tác, điều này sẽ tạo ra một cây khả năng lớn và nhiều trạng thái lặp lại theo các thứ tự cắt khác nhau. Tổng số cấu hình vượt xa những gì có thể liệt kê trong 10^4 trường hợp thử nghiệm. 

Quan sát quan trọng là chúng ta không thực sự quan tâm đến cấu hình trung gian mà chỉ quan tâm đến việc liệu có tồn tại độ dài cuối cùng thống nhất hay không. Nếu độ dài cuối cùng là x thì mọi đoạn ban đầu phải được phân tách thành các đoạn có độ dài x. Điều này biến vấn đề thành việc kiểm tra xem mỗi số có thể được biểu diễn dưới dạng tổng của các phần bằng nhau hay không, với hạn chế bổ sung là chúng ta chỉ có thể đưa ra tổng cộng tối đa ba lần cắt. 

Hạn chế này cực kỳ chặt chẽ: mỗi lần cắt tăng số lượng mảnh lên đúng một, vì vậy bắt đầu từ ba đoạn, chúng ta có thể kết thúc với tối đa sáu đoạn. Do đó, cấu hình cuối cùng có thể có kích thước 3, 4, 5 hoặc 6. Vì tất cả các phần phải có độ dài bằng nhau nên tổng S phải thỏa mãn S mod k = 0 đối với k trong {3,4,5,6}, trong đó k là số phần cuối cùng. Đối với mỗi k hợp lệ, chúng tôi kiểm tra xem mỗi phân đoạn gốc có thể được chia thành các số nguyên có kích thước S/k hay không và liệu tổng số lần cắt cần thiết có vượt quá 3 hay không. 

Đối với độ dài mục tiêu cố định x, một đoạn có độ dài a đóng góp một phần cắt bằng (a/x − 1) nếu a chia hết cho x, nếu không thì không thể. Tổng số này trên cả ba phân đoạn sẽ cho ra tổng số thao tác cần thiết. Câu trả lời là CÓ nếu tồn tại một số k khả thi trong [3,6] mang lại x hợp lệ và yêu cầu tối đa 3 lần cắt. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên tất cả các phần chia | Hàm mũ | Cao | Quá chậm | 
| Hãy thử tất cả số lượng mảnh cuối cùng (3-6) bằng cách kiểm tra khả năng chia hết | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi tiến hành bằng cách kiểm tra từng số mảnh cuối cùng có thể.

1. Tính tổng S của ba chiều dài ren. Đây là tổng chiều dài phải được chia đều cho tất cả các phần bằng nhau cuối cùng. 
2. Với mỗi số ứng viên của các quân cuối cùng k từ 3 đến 6, kiểm tra xem S có chia hết cho k hay không. Nếu không, k này không thể tạo ra các phân đoạn bằng nhau và bị loại bỏ. 
3. Với mỗi k hợp lệ, hãy tính chiều dài mảnh mục tiêu x = S / k. Đây là độ dài duy nhất có thể có của mỗi luồng cuối cùng. 
4. Với mỗi đoạn gốc a, kiểm tra xem a có chia hết cho x hay không. Nếu không, cấu hình này là không thể vì chúng ta không thể cắt thành các phần nhỏ. 
5. Nếu chia hết, hãy tính xem cần bao nhiêu lần cắt bên trong đoạn đó dưới dạng a / x − 1. Điều này xuất phát từ thực tế là việc chia a thành m phần bằng nhau cần chính xác m − 1 lần cắt. 
6. Tính tổng số lần cắt này trên cả ba đoạn. Nếu tổng số nhiều nhất là 3 thì cấu hình này có thể đạt được nên chúng ta có thể trả về CÓ ngay lập tức. 
7. Nếu không có k nào trong [3,6] hoạt động, trả về NO. 

### Tại sao nó hoạt động 

Mọi thao tác đều tăng số lượng phân đoạn lên đúng một, do đó, bắt đầu từ ba phân đoạn, chúng ta chỉ có thể đạt được cấu hình có tối đa sáu phân đoạn. Điều này giới hạn số lượng phần bằng nhau cuối cùng trong một phạm vi không đổi nhỏ. Đối với kích thước phân đoạn cuối cùng cố định, cấu trúc của các phần cắt bên trong mỗi phân đoạn ban đầu là bắt buộc: các phần bằng nhau hàm ý một độ dài mục tiêu duy nhất và mỗi phân đoạn xác định độc lập số lượng phần cắt mà nó đóng góp. Do các phân đoạn không tương tác ngoại trừ thông qua giới hạn độ dài cuối cùng được chia sẻ nên việc kiểm tra tính khả thi sẽ giảm xuống việc tổng hợp các chi phí độc lập và xác minh ngân sách cắt giảm toàn cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def possible(a, b, c):
    total = a + b + c

    for k in range(3, 7):
        if total % k != 0:
            continue
        x = total // k

        cuts = 0
        ok = True

        for v in (a, b, c):
            if v % x != 0:
                ok = False
                break
            cuts += v // x - 1

        if ok and cuts <= 3:
            return True

    return False

t = int(input())
for _ in range(t):
    a, b, c = map(int, input().split())
    print("YES" if possible(a, b, c) else "NO")
```Việc triển khai trực tiếp tuân theo quan sát rằng chỉ có kích thước phân vùng cuối cùng từ 3 đến 6 là phù hợp. Vòng lặp trên k liệt kê tất cả các trạng thái kết thúc có thể có và với mỗi trạng thái này chúng tôi xác minh xem liệu nó có thể được xây dựng với tối đa ba lần cắt hay không. 

Việc tính toán các vết cắt sử dụng đồng nhất thức việc chia một đoạn thành m phần bằng nhau yêu cầu chính xác các thao tác m−1. Điều này tránh việc mô phỏng quá trình phân tách một cách rõ ràng. Cờ boolean đảm bảo chúng tôi loại bỏ sớm các phân tách không hợp lệ khi khả năng chia hết không thành công. 

## Ví dụ đã hoạt động 

Xét trường hợp (6, 3, 3). Tổng số tiền là 12. 

| k | x = S/k | Tính chia hết hợp lệ | Cắt từ 6 | Cắt từ 3 | Tổng số cắt giảm | Kết quả | 
| --- | --- | --- | --- | --- | --- | --- | 
| 3 | 4 | vâng | 6/4 không hợp lệ | - | - | bỏ qua | 
| 4 | 3 | vâng | 6→3,3 = 1 | 3→3 = 0 | 1 | CÓ | 
| 5 | - | không | - | - | bỏ qua | | 
| 6 | 2 | vâng | 6→2,2,2 = 2 | 3 không hợp lệ | - | bỏ qua | 

Điều này chứng tỏ rằng k = 4 hiệu quả vì chúng ta có thể chia 6 thành hai phần của 3 (một phần cắt), trong khi các phần khác đã khớp hoặc không cần cắt. 

Bây giờ hãy xem xét (1, 3, 2). Tổng là 6. 

| k | x | hợp lệ | Cắt từ 1 | Cắt từ 3 | Cắt từ 2 | Tổng cộng | 
| --- | --- | --- | --- | --- | --- | --- | 
| 3 | 2 | vâng | không thể | 3→2+1 (1) | 2→2 (0) | không hợp lệ | 
| 4 | 1,5 | không | - | - | - | bỏ qua | 
| 5 | 1.2 | không | - | - | - | bỏ qua | 
| 6 | 1 | vâng | 0 | 2 | 1 | 3 CÓ | 

Điều này xác nhận rằng thuật toán chỉ tìm thấy cấu hình hợp lệ một cách chính xác khi tổng số lần cắt nằm trong giới hạn cho phép. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) mỗi lần kiểm tra | Chỉ có bốn giá trị ứng viên của k được kiểm tra, mỗi giá trị có công không đổi | 
| Không gian | O(1) | Không sử dụng cấu trúc phụ trợ | 

Số lượng thao tác không đổi trên mỗi trường hợp thử nghiệm đảm bảo giải pháp dễ dàng xử lý 10^4 đầu vào trong thời gian giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    def solve():
        a, b, c = map(int, input().split())
        total = a + b + c
        for k in range(3, 7):
            if total % k != 0:
                continue
            x = total // k
            cuts = 0
            ok = True
            for v in (a, b, c):
                if v % x != 0:
                    ok = False
                    break
                cuts += v // x - 1
            if ok and cuts <= 3:
                return "YES"
        return "NO"

    t = int(input())
    return "\n".join(solve() for _ in range(t))

# provided samples (partial check due to brevity)
assert run("1\n1 3 2\n") == "YES"
assert run("1\n5 5 5\n") == "YES"

# custom cases
assert run("1\n1 1 1\n") == "YES"  # already equal
assert run("1\n1 1 100\n") == "NO"  # impossible under 3 cuts
assert run("1\n6 3 3\n") == "YES"  # classic split case
assert run("1\n2 2 8\n") == "YES"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 1 | CÓ | đã thống nhất | 
| 1 1 100 | KHÔNG | mất cân bằng lớn không thể khắc phục trong 3 lần cắt | 
| 6 3 3 | CÓ | tính khả thi phân chia đa cấp | 
| 2 2 8 | CÓ | nhiều phân khúc góp phần cắt giảm | 

## Vỏ cạnh 

Trường hợp như (1, 1, 100) nhấn mạnh hạn chế là việc cắt giảm có giới hạn toàn cục. Thuật toán thử tất cả k trong [3,6], nhưng đối với bất kỳ x hợp lệ nào xuất phát từ các giá trị k này, phân đoạn lớn buộc phải chia quá nhiều hoặc phá vỡ khả năng chia hết, do đó tất cả các ứng cử viên đều thất bại và đầu ra là KHÔNG. 

Đối với (6, 3, 3), lời giải đúng yêu cầu nhận ra rằng chỉ cần tách một đoạn và phép chia đó tạo ra chính xác số phần phù hợp mà không vượt quá giới hạn thao tác. Thuật toán nắm bắt điều này bằng cách tính toán số lần cắt chính xác trên mỗi phân đoạn thay vì mô phỏng bất kỳ trình tự nào. 

Đối với các đầu vào đã bằng nhau như (5, 5, 5), trường hợp k = 3 với x = 5 mang lại mức cắt bằng 0, ngay lập tức thỏa mãn ràng buộc, tạo ra CÓ mà không thực hiện bất kỳ thao tác nào.
