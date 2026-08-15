---
title: "CF 102426B - Bí mật của thời gian"
description: "Không có đầu vào nào cả. Chúng ta chỉ cần in ra một số nguyên dương (x) có bình phương là số thập phân có 16 chữ số thỏa mãn điều kiện có nhiều chữ số."
date: "2026-08-14T15:27:26+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102426
codeforces_index: "B"
codeforces_contest_name: "The 7-th BIT Campus Programming Contest for Junior Grade Group"
rating: 0
weight: 102426
solve_time_s: 218
verified: true
draft: false
---

[CF 102426B - Bí mật của thời gian](https://codeforces.com/problemset/problem/102426/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3 phút 38 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Không có đầu vào nào cả. Chúng ta chỉ cần in ra một số nguyên dương (x) có bình phương là số thập phân có 16 chữ số thỏa mãn điều kiện có nhiều chữ số. 

Đếm các chữ số từ bên phải, hình vuông phải có chữ số 1 ở vị trí 1 và 13, chữ số 9 ở vị trí 3, chữ số 2 ở vị trí 5, chữ số 6 ở vị trí 7, chữ số 0 ở vị trí 9, chữ số 8 ở vị trí 11 và chữ số 7 ở vị trí 15. Tám chữ số còn lại không bị hạn chế. Vì tuyên bố rõ ràng cho phép bất kỳ mật khẩu hợp lệ nào nên việc tìm một nhân chứng là đủ. 

Cách hữu ích nhất để xem điều kiện là dưới dạng ràng buộc về biểu diễn thập phân của (x^2). Từ bên trái, một hình vuông hợp lệ có dạng 

[ 
7_1_8_0_6_2_9_1. 
] 

Chữ số cố định hàng đầu cho một giới hạn mạnh mẽ đáng ngạc nhiên. Vì hai chữ số đầu tiên là (7) nên hình vuông nằm trong khoảng 

[ 
7\cdot10^{15} \le x^2 < 8\cdot10^{15}. 
] 

Như vậy (x) nằm giữa (\sqrt{7\cdot10^{15}}) và (\sqrt{8\cdot10^{15}}), khoảng từ 83,7 triệu đến 89,4 triệu. Chỉ có khoảng 5,8 triệu căn là có liên quan, chứ không phải mọi số nguyên có bình phương có 16 chữ số. 

Không có trường hợp cạnh đầu vào thông thường vì luồng đầu vào trống theo định nghĩa. Việc triển khai vẫn phải xử lý luồng trống một cách chính xác thay vì cố đọc số nguyên và chặn hoặc không thành công. Một lỗi dễ mắc phải khác là diễn giải các chữ số cố định từ bên trái thay vì bên phải. Ví dụ: một hình vuông như`701080006020901`có chuỗi con hiển thị`19260817`được nhúng trong cấu trúc thập phân của nó, nhưng nó chỉ có 15 chữ số và không phải là mục tiêu hợp lệ. Các vị trí phải được tính từ chữ số có nghĩa nhỏ nhất. 

Lỗi phổ biến thứ hai là chỉ kiểm tra các chữ số cố định nhìn thấy được và quên rằng kết quả thực sự phải là một số chính phương. Ví dụ: một số phù hợp với mẫu`7_1_8_0_6_2_9_1`không được tự động chấp nhận. Chương trình phải bắt đầu từ một nghiệm dự kiến, bình phương nó và sau đó xác minh vị trí các chữ số. 

## Phương pháp tiếp cận 

Lực lượng vũ phu trực tiếp là khái niệm đơn giản. Liệt kê mọi số nguyên dương có bình phương có 16 chữ số, tính bình phương của nó, chuyển đổi thành số thập phân và kiểm tra tám vị trí cố định. Gốc đầu tiên có thể là (\lceil\sqrt{10^{15}}\rceil=31,622,777), trong khi lớn nhất là (\lfloor\sqrt{10^{16}-1}\rfloor=99,999,999). Điều đó mang lại 68.377.223 ứng viên. Mỗi ứng cử viên yêu cầu một phép nhân và kiểm tra chữ số, số lượng này lớn không cần thiết đối với một bài toán có giới hạn một giây. 

Cấu trúc của các ràng buộc thập phân cho chúng ta bộ lọc tốt hơn nhiều. Chữ số cố định cuối cùng là 1, do đó gốc phải kết thúc bằng 1 hoặc 9. Mạnh mẽ hơn, chữ số thứ ba từ bên phải của hình vuông phải là 9. Điều kiện đó chỉ phụ thuộc vào gốc modulo (1000), vì (x^2\bmod1000) chỉ phụ thuộc vào (x\bmod1000). 

Chúng ta có thể liệt kê 1000 số dư có thể có theo modulo 1000 một lần và chỉ giữ lại các số dư có bình phương có chữ số hàng đơn vị là 1 và chữ số hàng trăm là 9. Điều này chỉ để lại một tập hợp nhỏ các hậu tố có thể có cho nghiệm. 

Chúng ta cũng có thể sử dụng ràng buộc chữ số hàng đầu trước khi tìm kiếm. Chữ số thứ hai bắt buộc tính từ bên trái là 7, vì vậy bình phương nằm giữa (7\cdot10^{15}) và (8\cdot10^{15}). Chúng tôi tính toán các giới hạn căn bậc hai đó một cách chính xác với`math.isqrt`. 

Tìm kiếm brute-force giờ đây đã trở thành một tìm kiếm được lọc kỹ lưỡng. Thay vì kiểm tra hàng chục triệu nghiệm, chúng ta chỉ kiểm tra các nghiệm trong dãy dẫn đầu hẹp có ba chữ số cuối cùng đã thỏa mãn hai điều kiện cố định. Với mỗi căn như vậy, chúng ta tính bình phương của nó và kiểm tra các vị trí còn lại. Chỉ có khoảng vài trăm nghìn ứng viên, vì vậy con số này khá nhỏ trong Python. 

Cách tiếp cận brute-force hoạt động vì không gian tìm kiếm là hữu hạn và mọi ứng cử viên đều được kiểm tra trực tiếp, nhưng nó lãng phí gần như toàn bộ thời gian đối với các nghiệm có ba chữ số cuối khiến câu trả lời cuối cùng là không thể. Việc quan sát rằng các vị trí thập phân được xác định lũy thừa modulo của mười cho phép chúng tôi loại bỏ các gốc đó trước khi thực hiện xác nhận đầy đủ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(10^8)) | (O(1)) | Quá chậm | 
| Tối ưu | (O(10^6)) trường hợp xấu nhất, với hằng số nhỏ | (O(1)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính số nguyên nhỏ nhất có bình phương nhỏ nhất là (7\cdot10^{15}) và số nguyên lớn nhất có bình phương nhỏ hơn (8\cdot10^{15}). Đây chính xác là những căn bậc có bình phương có thể có hai chữ số đầu tiên theo yêu cầu. 
2. Liệt kê mọi số dư (r) từ 0 đến 999. Tính (r^2\bmod1000) và giữ lại (r) nếu chữ số hàng đơn vị là 1 và chữ số hàng trăm là 9. Bất kỳ nghiệm hợp lệ nào cũng phải có một trong các số dư này làm ba chữ số cuối cùng. 
3. Với mỗi phần dư được giữ lại, hãy tìm nghiệm đầu tiên trong khoảng cho phép có phần dư đó modulo 1000. Sau đó mỗi lần tăng nghiệm lên 1000. Điều này sẽ truy cập mọi gốc có hậu tố đó chính xác một lần. 
4. Bình phương mỗi ứng cử viên. Hình vuông đã có phạm vi dẫn đầu được yêu cầu và ba chữ số cuối cùng của nó thỏa mãn hai điều kiện cố định thấp nhất, do đó chỉ cần kiểm tra các vị trí cố định còn lại. 
5. Chuyển đổi hình vuông thành chuỗi thập phân và kiểm tra các vị trí 5, 7, 9, 11, 13 và 15 từ bên phải. Khi tất cả sáu vị trí khớp nhau, hãy in phần gốc và dừng lại. 
6. Bởi vì vấn đề đảm bảo rằng mật khẩu tồn tại và mọi ứng cử viên có thể có trong tìm kiếm được lọc đều được kiểm tra, tìm kiếm cuối cùng sẽ đạt đến một gốc hợp lệ. 

Bất biến chính là mọi gốc bị thuật toán bỏ qua là không thể. Một căn nằm ngoài khoảng đầu không thể tạo ra một hình vuông bắt đầu bằng 7. Một căn có phần dư modulo 1000 bị loại bỏ không thể tạo ra một hình vuông kết thúc bằng đơn vị được yêu cầu và các chữ số hàng trăm. Mọi gốc còn lại đều được kiểm tra trực tiếp với mọi vị trí cố định khác. Do đó, gốc đầu tiên được thuật toán in ra nhất thiết phải là mật khẩu hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

from math import isqrt

def find_answer():
    # The square must start with digit 7 and have exactly 16 digits.
    lo = isqrt(7 * 10**15 - 1) + 1
    hi = isqrt(8 * 10**15 - 1)

    # A valid square has:
    # position 1 from the right = 1
    # position 3 from the right = 9
    #
    # Both conditions depend only on x modulo 1000.
    residues = []
    for r in range(1000):
        v = r * r % 1000
        if v % 10 == 1 and (v // 100) % 10 == 9:
            residues.append(r)

    # Remaining fixed digits, indexed by their position from the right.
    required = {
        4: 2,   # position 5
        6: 6,   # position 7
        8: 0,   # position 9
        10: 8,  # position 11
        12: 1,  # position 13
        14: 7,  # position 15
    }

    for r in residues:
        # First x >= lo such that x % 1000 == r.
        x = lo + (r - lo) % 1000

        while x <= hi:
            sq = x * x
            s = str(sq)

            ok = True
            for pos, digit in required.items():
                if s[-1 - pos] != str(digit):
                    ok = False
                    break

            if ok:
                return x

            x += 1000

    raise RuntimeError("A valid password was not found")

def solve():
    print(find_answer())

if __name__ == "__main__":
    solve()
```Đầu tiên, chương trình lấy khoảng gốc thay vì mã hóa giới hạn gần đúng.`isqrt`thực hiện căn bậc hai số nguyên chính xác, do đó không có rủi ro làm tròn dấu phẩy động gần điểm cuối. 

Việc sử dụng dư lượng xây dựng`r * r % 1000`. biểu thức`v % 10`kiểm tra chữ số hàng đơn vị, trong khi`(v // 100) % 10`trích ra chữ số hàng trăm. Vì bình phương modulo 1000 được xác định hoàn toàn bằng ba chữ số cuối của căn nó nên việc loại bỏ mọi phần dư khác là an toàn về mặt toán học. 

Đối với mỗi dư lượng còn sót lại,`(r - lo) % 1000`tính toán mức điều chỉnh không âm nhỏ nhất cần thiết để đạt được phần dư đó. Thêm 1000 sau đó sẽ giữ nguyên số dư, do đó không có ứng cử viên nào bị bỏ qua. 

Từ điển`required`sử dụng các vị trí dựa trên số 0 từ bên phải. Vị trí 5 trong bài toán là chỉ số 4 trong biểu diễn này, do đó quyền truy cập chuỗi là`s[-1 - pos]`. Việc lập chỉ mục này là nơi thường xảy ra lỗi từng cái một. Hai vị trí đã được đảm bảo bởi bộ lọc cặn được cố tình bỏ qua trong lần kiểm tra thứ hai này. 

Số nguyên Python có độ chính xác tùy ý, do đó hình vuông vừa vặn mà không có bất kỳ vấn đề tràn nào. Căn bậc cao nhất có liên quan là dưới 90 triệu và bình phương của nó nhỏ hơn (10^{16}). 

## Ví dụ đã hoạt động 

Không có đầu vào mẫu thông thường vì bài toán ban đầu không có đầu vào. Các dấu vết sau đây minh họa quá trình lọc trên hai gốc ứng cử viên khác nhau. Ứng viên đầu tiên bị từ chối vì một trong các chữ số ở giữa bắt buộc bị sai, trong khi loại dấu vết thứ hai thể hiện đường dẫn thành công khi mọi chữ số cố định đều đồng ý. 

Đối với thí sinh có ba chữ số cuối là 089, bài kiểm tra dư lượng thành công vì 

[ 
89^2=7921, 
] 

nên hình vuông có chữ số hàng đơn vị là 1 và chữ số hàng trăm là 9. 

| Sân khấu | Hậu tố gốc | Hậu tố vuông | Kết quả | 
| --- | --- | --- | --- | 
| Kiểm tra dư lượng | 089 | 921 | Vượt qua | 
| Vị trí 5 | 2 | 2 | Vượt qua | 
| Vị trí 7 | 6 | 6 | Vượt qua | 
| Vị trí 9 | 0 | 9 | Thất bại | 

Điều này chứng tỏ tại sao chỉ kiểm tra hậu tố là không đủ. Một ứng cử viên có thể đáp ứng các ràng buộc bậc thấp trong khi không đạt được vị trí thập phân cao hơn. 

Đối với một ứng cử viên vượt qua tất cả các bộ lọc, lần xác minh cuối cùng sẽ kiểm tra hình vuông hoàn chỉnh gồm 16 chữ số. 

| Vị trí từ phải | Chữ số bắt buộc | chữ số ứng cử viên | Kết quả | 
| --- | --- | --- | --- | 
| 1 | 1 | 1 | Vượt qua | 
| 3 | 9 | 9 | Vượt qua | 
| 5 | 2 | 2 | Vượt qua | 
| 7 | 6 | 6 | Vượt qua | 
| 9 | 0 | 0 | Vượt qua | 
| 11 | 8 | 8 | Vượt qua | 
| 13 | 1 | 1 | Vượt qua | 
| 15 | 7 | 7 | Vượt qua | 

Dấu vết thứ hai xác nhận bất biến được sử dụng bởi tìm kiếm. Mọi vị trí bắt buộc đều được kiểm tra độc lập, do đó, một ứng cử viên chỉ được in sau khi toàn bộ mẫu đã được xác minh. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(10^6)) trong tìm kiếm đã lọc | Chỉ các gốc trong khoảng đầu với hậu tố modulo-1000 hợp lệ mới được kiểm tra | 
| Không gian | (O(1)) | Danh sách dư lượng chứa tối đa 1000 mục và tất cả trạng thái khác có kích thước không đổi | 

Dãy ô vuông 16 chữ số ban đầu chứa hàng chục triệu nghiệm có thể có. Việc giới hạn bình phương ở mẫu dẫn đầu làm giảm con số này xuống còn khoảng 5,8 triệu nghiệm và việc hạn chế nghiệm modulo 1000 làm giảm các ứng cử viên thực tế được kiểm tra theo hệ số khác từ khoảng 10 đến 100, tùy thuộc vào số dư còn sót lại. Tìm kiếm kết quả đủ nhỏ cho giới hạn một giây và sử dụng bộ nhớ không đáng kể. 

## Trường hợp thử nghiệm 

Bởi vì vấn đề ban đầu không có đầu vào nên các bài kiểm tra có ý nghĩa sẽ xác thực mật khẩu được tạo thay vì so sánh nó với đầu ra mẫu cố định. Cho phép nhiều mật khẩu hợp lệ, vì vậy việc kiểm tra một câu trả lời bằng số cụ thể sẽ bị hạn chế một cách không cần thiết.```python
# helper: run the solver on an input string and return its output
import sys
import io
from math import isqrt

def find_answer():
    lo = isqrt(7 * 10**15 - 1) + 1
    hi = isqrt(8 * 10**15 - 1)

    residues = []
    for r in range(1000):
        v = r * r % 1000
        if v % 10 == 1 and (v // 100) % 10 == 9:
            residues.append(r)

    required = {
        4: 2,
        6: 6,
        8: 0,
        10: 8,
        12: 1,
        14: 7,
    }

    for r in residues:
        x = lo + (r - lo) % 1000

        while x <= hi:
            sq = x * x
            s = str(sq)

            if all(s[-1 - pos] == str(digit)
                   for pos, digit in required.items()):
                return x

            x += 1000

    raise RuntimeError("No answer found")

def solve():
    print(find_answer())

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    try:
        sys.stdin = io.StringIO(inp)
        sys.stdout = io.StringIO()
        solve()
        return sys.stdout.getvalue().strip()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

def valid_password(x: int) -> bool:
    sq = x * x
    s = str(sq)

    if len(s) != 16:
        return False

    required = {
        0: '1',
        2: '9',
        4: '2',
        6: '6',
        8: '0',
        10: '8',
        12: '1',
        14: '7',
    }

    return all(s[-1 - pos] == digit
               for pos, digit in required.items())

# Provided sample: the problem has no input.
answer = int(run(""))
assert valid_password(answer), "sample 1"

# Empty input with a trailing newline.
answer = int(run("\n"))
assert valid_password(answer), "empty input"

# Whitespace-only input.
answer = int(run("   \n\n"))
assert valid_password(answer), "whitespace input"

# Repeated empty invocations must still produce a valid password.
answer = int(run(""))
assert valid_password(answer), "repeated empty input"

# Boundary-oriented validation.
answer = int(run("\n\n\n"))
sq = answer * answer
assert 7 * 10**15 <= sq < 8 * 10**15, "leading digit constraint"
assert len(str(sq)) == 16, "16-digit square"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Đầu vào trống | Bất kỳ mật khẩu hợp lệ nào | Định dạng mẫu được cung cấp | 
|`\n`| Bất kỳ mật khẩu hợp lệ nào | Đầu vào trống với dòng mới | 
| Chỉ khoảng trắng | Bất kỳ mật khẩu hợp lệ nào | Giải pháp không phụ thuộc vào phân tích cú pháp đầu vào | 
| Một số dòng mới | Bất kỳ mật khẩu hợp lệ nào | Hành vi nhập trống lặp đi lặp lại | 
| Đầu vào trống với các kiểm tra vuông rõ ràng | Bất kỳ mật khẩu hợp lệ nào | Phạm vi dẫn đầu, độ dài và tất cả các chữ số cố định | 

## Vỏ cạnh 

Trường hợp cạnh đầu tiên là chính đầu vào trống. Chạy chương trình không có dữ liệu vẫn phải tạo mật khẩu. Việc thực hiện không bao giờ gọi`input()`vì không có gì để đọc nên luồng trống được xử lý một cách tự nhiên. 

Trường hợp cạnh thứ hai là một nghiệm có ba chữ số cuối trông có vẻ hứa hẹn nhưng sau đó bình phương của nó lại thất bại. Một hậu tố như`089`tạo ra một hình vuông kết thúc bằng`921`, thỏa mãn chữ số hàng đơn vị 1 và chữ số hàng trăm 9. Việc triển khai bất cẩn có thể dừng ở đó, nhưng mẫu hoàn chỉnh cũng yêu cầu chữ số thứ chín tính từ bên phải là 0. Việc kiểm tra hình vuông đầy đủ sẽ phát hiện ra lỗi này. 

Trường hợp cạnh thứ ba liên quan đến ranh giới phía trên. Một hình vuông bắt đầu bằng 6 hoặc 8 không thể đáp ứng chữ số thứ hai bắt buộc tính từ bên trái, bất kể các chữ số thấp hơn của nó khớp hoàn hảo đến mức nào. Tính toán khoảng thời gian từ (7\cdot10^{15}) đến (8\cdot10^{15}-1) sẽ loại bỏ các ứng cử viên này trước khi xác minh tốn kém. 

Trường hợp cạnh thứ tư là chữ số 0 ở vị trí 9. Vì số 0 là chữ số thập phân hợp lệ nên việc triển khai phải so sánh nó một cách rõ ràng thay vì coi nó là chữ số vắng mặt. Kiểm tra chuỗi sử dụng ký tự`'0'`, do đó, một hình vuông có bất kỳ chữ số nào khác ở vị trí đó sẽ bị từ chối.
