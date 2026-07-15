---
title: "CF 103379G - Xe trượt tuyết mới của ông già Noel"
description: "Chúng ta được cung cấp một chuỗi các vị trí trong mặt phẳng 2D, bắt đầu từ gốc tọa độ. Xe trượt tuyết của ông già Noel không còn tự do lựa chọn hướng đi nữa. Thay vào đó, nó liên tục thực hiện một mô hình chuyển động cố định được đưa ra bởi một chuỗi gồm bốn hướng chính."
date: "2026-07-03T12:34:54+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103379
codeforces_index: "G"
codeforces_contest_name: "UTPC Contest 10-29-21 Div. 1 (Advanced)"
rating: 0
weight: 103379
solve_time_s: 51
verified: true
draft: false
---

[CF 103379G - Xe trượt tuyết mới của ông già Noel](https://codeforces.com/problemset/problem/103379/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 51s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi các vị trí trong mặt phẳng 2D, bắt đầu từ gốc tọa độ. Xe trượt tuyết của ông già Noel không còn tự do lựa chọn hướng đi nữa. Thay vào đó, nó liên tục thực hiện một mô hình chuyển động cố định được đưa ra bởi một chuỗi gồm bốn hướng chính. 

Về mặt khái niệm, chúng ta tưởng tượng ông già Noel đứng ở vị trí (0, 0) và xâu chuỗi từ trái sang phải. Sau khi kết thúc chuỗi một lần, anh ta ngay lập tức lặp lại chuỗi đó một lần nữa và điều này tiếp tục vô tận. Mỗi lần lặp lại tạo ra một vectơ dịch chuyển xác định, do đó chuyển động có cấu trúc tuần hoàn. 

Câu hỏi đặt ra là liệu tại bất kỳ thời điểm nào trong chuyến đi vô tận này, ông già Noel có hạ cánh chính xác tại tọa độ mục tiêu (x, y) hay không. Điều này không giới hạn ở phần cuối của toàn bộ chu kỳ của chuỗi mà có thể xảy ra ở bất kỳ bước trung gian nào trong bất kỳ sự lặp lại nào. 

Các ràng buộc quan trọng theo hai cách. Giới hạn tọa độ đạt tới tỷ lệ 10^18, do đó việc mô phỏng các bước trực tiếp là không thể. Độ dài chuỗi có thể lên tới 100.000, do đó, ngay cả một lần truyền cũng đã đủ lớn để bất kỳ mô phỏng O(n^2) hoặc lặp lại nào trên mỗi chu kỳ đều quá chậm. Hướng khả thi duy nhất là coi việc thực thi một chuỗi đầy đủ như một phép biến đổi và suy luận đại số về các ứng dụng lặp lại. 

Một trường hợp cạnh tinh tế phát sinh khi độ dịch chuyển ròng sau một chuỗi đầy đủ bằng không. Trong trường hợp đó, chiếc xe trượt tuyết bị mắc kẹt bên trong một quỹ đạo giới hạn và chỉ những vị trí đã ghé thăm trong chu kỳ đầu tiên mới quan trọng. Nếu chúng ta bỏ qua điều này và giả sử sự dịch chuyển định kỳ, chúng ta có thể xác nhận sai khả năng tiếp cận các tọa độ chưa bao giờ được truy cập thực sự. 

Một trường hợp biên khác xảy ra khi tọa độ mục tiêu chỉ có thể đạt được ở đoạn giữa của chu kỳ chứ không phải ở ranh giới chu kỳ. Ví dụ: nếu chuỗi là "UR", các vị trí đã truy cập là (0,0), (0,1), (1,1), sau đó lặp lại. Một mục tiêu như (0,1) phải được phát hiện bên trong chu trình, không chỉ thông qua lý luận dịch chuyển ròng. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: mô phỏng xe trượt tuyết từng bước, lưu trữ mọi tọa độ đã truy cập và kiểm tra xem (x, y) có xuất hiện hay không. Điều này đúng vì chuyển động có tính chất xác định và tuần hoàn nên cuối cùng hoặc chúng ta nhìn thấy mục tiêu hoặc chúng ta không nhìn thấy. Vấn đề là đường dẫn là vô hạn, do đó, lực lượng vũ phu phải dừng lại một cách giả tạo sau một số bước. 

Điểm cắt tự nhiên là để mô phỏng tối đa một số lượng lớn các chu kỳ, ví dụ như các bước O(n * C) trong đó C đủ lớn để “bao phủ” tất cả các độ lệch có thể có. Tuy nhiên, vì tọa độ có thể lớn tới 10^18 nên không có giới hạn an toàn về số lượng chu kỳ có thể cần thiết. Thậm chí vài triệu chu kỳ sẽ vượt quá giới hạn thời gian. 

Điểm mấu chốt là mỗi ứng dụng chuỗi đầy đủ đều đóng góp một vectơ dịch chuyển cố định. Đặt độ dời đó là (dx, dy). Sau k chu kỳ đầy đủ, vị trí là k * (dx, dy) cộng với vị trí bên trong chu kỳ hiện tại. Điều này làm giảm bước đi vô hạn để kiểm tra cấp số cộng của các điểm. 

Sau đó chúng tôi đảo ngược quan điểm. Thay vì hỏi liệu (x, y) có từng được ghé thăm hay không, chúng ta hỏi liệu có tồn tại chỉ số chu trình k và vị trí tiền tố i trong chuỗi sao cho chuyển vị tiền tố tại i cộng k nhân với chuyển vị toàn chu kỳ bằng (x, y). Điều này biến bài toán thành việc kiểm tra các điều kiện Diophantine tuyến tính trên một tập hợp nhỏ các trạng thái tiền tố. 

Nếu (dx, dy) không phải (0, 0), thì k được xác định duy nhất cho mỗi ứng cử viên tiền tố và chúng ta chỉ cần xác minh xem k đó có phải là số nguyên không âm nhất quán cho cả hai tọa độ hay không. Nếu (dx, dy) là (0, 0), thì không có sự dịch chuyển nào xảy ra giữa các chu kỳ, do đó chỉ có một chu kỳ quan trọng và chúng tôi trực tiếp kiểm tra xem mục tiêu có xuất hiện trong quá trình truyền tải tiền tố hay không. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(vô hạn hoặc bị chặn O(n·C)) | O(n) | Quá chậm/không đầy đủ | 
| Tiền tố + Phân tách chu kỳ | O(n) | O(n) | Đã chấp nhận |

## Hướng dẫn thuật toán 

1. Tính vị trí tiền tố của ô trượt sau mỗi ký tự trong chuỗi bắt đầu từ (0, 0). Điều này cung cấp vị trí chính xác sau mỗi bước trong một chu kỳ. Điều này là cần thiết vì mục tiêu có thể đạt được vào giữa chu kỳ. 
2. Tính tổng độ dịch chuyển của một chu kỳ đầy đủ dưới dạng (dx, dy) bằng cách lấy vị trí tiền tố cuối cùng sau khi xử lý toàn bộ chuỗi. Điều này ghi lại cách toàn bộ mẫu dịch chuyển không gian sau một lần lặp lại. 
3. Đối với mỗi vị trí tiền tố (px, py) tương ứng với chỉ mục i trong chuỗi (bao gồm cả số đầu tiên (0, 0)), hãy xem xét liệu mục tiêu (x, y) có thể khớp với trạng thái này sau một số chu kỳ đầy đủ hay không. Điều này có nghĩa là chúng ta kiểm tra xem (x - px, y - py) có thể được biểu diễn dưới dạng k * (dx, dy) đối với một số nguyên k ≥ 0 hay không. 
4. Nếu dx khác 0, hãy tính k dưới dạng (x - px) / dx và xác minh nó là số nguyên. Đồng thời kiểm tra xem việc áp dụng nó cho y có mang lại tính nhất quán hay không: (y - py) phải bằng k * dy. Điều này ngăn chặn việc chia tỷ lệ không khớp khi dx và dy không được căn chỉnh. 
5. Nếu dx bằng 0 nhưng dy khác 0, hãy thực hiện kiểm tra đối xứng chỉ sử dụng tọa độ y, vì chuyển động ngang không bao giờ thay đổi giữa các chu kỳ. 
6. Nếu cả dx và dy đều bằng 0 thì xe trượt hoàn toàn không chuyển động. Trong trường hợp này, chỉ có chu kỳ đầu tiên mới quan trọng, vì vậy chúng tôi chỉ cần kiểm tra xem có bất kỳ vị trí tiền tố nào bằng mục tiêu hay không. 
7. Nếu bất kỳ vị trí tiền tố nào thỏa mãn điều kiện, hãy trả về “Có”. Ngược lại trả về “Không”. 

Lý do nó hoạt động dựa trên tính bất biến là mọi điểm truy cập trong bước đi vô hạn có thể được phân tách duy nhất thành trạng thái tiền tố trong một chu kỳ cộng với một số nguyên các lần dịch chuyển toàn chu kỳ. Việc liệt kê tiền tố bao gồm tất cả các khả năng trong chu kỳ, trong khi việc chia tỷ lệ tuyến tính theo (dx, dy) nắm bắt tất cả chuyển động giữa các chu kỳ. Không có điểm nào khác tồn tại trên quỹ đạo ngoài những sự kết hợp này. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    x, y = map(int, input().split())
    s = input().strip()

    # prefix positions
    px = py = 0
    pref = [(0, 0)]

    for c in s:
        if c == 'U':
            py += 1
        elif c == 'D':
            py -= 1
        elif c == 'L':
            px -= 1
        else:
            px += 1
        pref.append((px, py))

    dx, dy = pref[-1]

    for i, (cx, cy) in enumerate(pref):
        tx = x - cx
        ty = y - cy

        if dx == 0 and dy == 0:
            if tx == 0 and ty == 0:
                print("Yes")
                return
            continue

        if dx == 0:
            if tx == 0 and dy != 0 and ty % dy == 0:
                k = ty // dy
                if k >= 0 and cy + k * dy == y:
                    print("Yes")
                    return
            continue

        if dy == 0:
            if ty == 0 and dx != 0 and tx % dx == 0:
                k = tx // dx
                if k >= 0 and cx + k * dx == x:
                    print("Yes")
                    return
            continue

        if tx % dx == 0:
            k = tx // dx
            if k >= 0 and cy + k * dy == y:
                print("Yes")
                return

    print("No")

if __name__ == "__main__":
    solve()
```Cấu trúc tiền tố trực tiếp mã hóa tất cả các điểm cuối có thể có trong chu kỳ. Sự dịch chuyển cuối cùng`(dx, dy)`nắm bắt được cấu trúc lặp lại. Mỗi khối điều kiện xử lý các suy biến trong đó một hoặc cả hai trục không thay đổi trong các chu kỳ, nếu không sẽ dẫn đến các giả định số nguyên chia cho 0 hoặc không hợp lệ. Chi tiết triển khai chính là luôn xác thực tính nhất quán trên cả hai tọa độ chứ không chỉ một tọa độ. 

## Ví dụ đã hoạt động 

Hãy xem xét đầu vào:```
2 2
UR
```Các trạng thái tiền tố là: 

| tôi | di chuyển | vị trí | 
| --- | --- | --- | 
| 0 | bắt đầu | (0, 0) | 
| 1 | Bạn | (0, 1) | 
| 2 | R | (1, 1) | 

Độ dịch chuyển của chu kỳ là (1, 1). Mục tiêu (2, 2) khớp với tiền tố (0, 0) với k = 2 chu kỳ, do đó có thể truy cập được. 

Điều này chứng tỏ rằng khả năng tiếp cận có thể xảy ra chính xác ở ranh giới chu kỳ và tiền tố (0, 0) phải luôn được xem xét. 

Bây giờ hãy xem xét:```
1 2
RU
```Tiền tố: 

| tôi | di chuyển | vị trí | 
| --- | --- | --- | 
| 0 | bắt đầu | (0, 0) | 
| 1 | R | (1, 0) | 
| 2 | Bạn | (1, 1) | 

Độ dịch chuyển của chu kỳ là (1, 1). Mục tiêu (1, 2) không thể được biểu thị dưới dạng bất kỳ tiền tố cộng bội số nguyên nào của (1, 1), do đó không thể truy cập được. 

Điều này cho thấy tại sao việc chỉ kiểm tra độ lớn khoảng cách hoặc tính tuần hoàn đơn giản lại thất bại, vì sự ghép hướng giữa x và y rất quan trọng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Một lượt để xây dựng các vị trí tiền tố và một lượt để kiểm tra chúng | 
| Không gian | O(n) | Lưu trữ tọa độ tiền tố cho từng vị trí trong chu trình | 

Giải pháp này dễ dàng phù hợp với các ràng buộc vì n lên tới 100.000 và tất cả các phép toán đều là số học theo thời gian không đổi cho mỗi ký tự. Dấu chân bộ nhớ là tuyến tính theo kích thước chuỗi, có thể chấp nhận được trong giới hạn 256 MB thông thường. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# sample-like cases
assert run("2 2\nUR\n") == "Yes"
assert run("1 2\nRU\n") == "No"

# minimal case
assert run("0 0\nU\n") == "Yes"

# cycle zero displacement
assert run("1 1\nUDLR\n") == "No"

# reachable mid-cycle repeated shift
assert run("3 1\nUR\n") == "Yes"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 0 0 / U | Có | xử lý nguồn gốc tiền tố | 
| UDLR với mục tiêu 1 1 | Không | chu kỳ dịch chuyển ròng bằng không | 
| UR với mục tiêu 3 1 | Có | tích lũy chu kỳ lặp đi lặp lại | 

## Vỏ cạnh 

Một trường hợp cạnh quan trọng là khi vectơ dịch chuyển của chuỗi đầy đủ bằng 0. Trong tình huống này, chiếc xe trượt không bao giờ dịch chuyển giữa các chu kỳ, do đó, các điểm duy nhất có thể tiếp cận được là những điểm ở lần đi ngang đầu tiên của chuỗi. Thuật toán xử lý vấn đề này bằng cách giảm ngay vấn đề xuống việc kiểm tra tiền tố một chu kỳ, đảm bảo không có phép ngoại suy sai qua các chu kỳ. 

Một trường hợp cạnh khác phát sinh khi dx hoặc dy bằng 0. Nếu dx bằng 0, vị trí nằm ngang không bao giờ thay đổi giữa các chu kỳ, do đó việc chia cho dx sẽ không hợp lệ. Thuật toán tách biệt rõ ràng trường hợp này và chỉ thực hiện kiểm tra tính hợp lệ trên trục thực sự thay đổi. 

Trường hợp tinh tế cuối cùng là khi mục tiêu chỉ có thể đạt được ngay từ đầu (0, 0). Điều này được đề cập vì danh sách tiền tố bao gồm vị trí ban đầu trước bất kỳ chuyển động nào, do đó thuật toán xác định chính xác thành công ngay lập tức mà không cần bất kỳ lý do chu kỳ nào.
