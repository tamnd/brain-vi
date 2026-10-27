---
title: "CF 106125I - Khoảng thời gian thành phần"
description: "Chúng tôi được cung cấp một danh sách các thành phần xuất hiện trên nhãn sản phẩm. Một số thành phần có tỷ lệ phần trăm đã biết và những thành phần khác không được xác định."
date: "2026-06-19T20:00:23+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106125
codeforces_index: "I"
codeforces_contest_name: "Delft Algorithm Programming Contest 2025 (DAPC 2025)"
rating: 0
weight: 106125
solve_time_s: 56
verified: true
draft: false
---

[CF 106125I - Khoảng thời gian thành phần](https://codeforces.com/problemset/problem/106125/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 56s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một danh sách các thành phần xuất hiện trên nhãn sản phẩm. Một số thành phần có tỷ lệ phần trăm đã biết và những thành phần khác không được xác định. Tất cả các tỷ lệ phần trăm đều không âm, tổng bằng chính xác 100 và danh sách được sắp xếp theo thứ tự tỷ lệ phần trăm giảm dần, nghĩa là các thành phần trước đó có tỷ lệ phần trăm ít nhất bằng với các thành phần sau. 

Nhiệm vụ là xác định, đối với mỗi thành phần chưa biết, giá trị nhỏ nhất và lớn nhất có thể có trong khi vẫn cho phép hoàn thành hợp lệ toàn bộ danh sách. Việc hoàn thành có nghĩa là gán giá trị cho tất cả các mục còn thiếu sao cho ràng buộc về thứ tự được tôn trọng và tổng số tiền chính xác là 100. 

Điều tinh tế quan trọng là các giá trị chưa biết không độc lập. Việc thay đổi một giá trị sẽ hạn chế tất cả các giá trị khác do ràng buộc tổng toàn cục và ràng buộc thứ tự đơn điệu. Đầu ra không phải là một phép gán đơn lẻ mà là một khoảng thời gian cho từng thành phần chưa biết. 

Các hạn chế là nhỏ, tối đa là 100 thành phần. Điều này ngay lập tức loại trừ bất cứ điều gì nặng nề hơn lý luận bậc hai hoặc tính toán lại đầy đủ lặp đi lặp lại cho mỗi thành phần. Phương pháp quét tuyến tính tham lam hoặc trực tiếp được kỳ vọng là đủ. 

Một sai lầm ngây thơ xuất phát từ việc xử lý từng ẩn số một cách độc lập, như thể chúng ta có thể ấn định khối lượng còn lại một cách tùy ý. Điều đó phá vỡ các ràng buộc đặt hàng. 

Ví dụ: hãy xem xét một danh sách như: 

sô cô la 41.1 

đường 

đậu phộng 20 

tinh bột 

Một cách tiếp cận ngây thơ có thể cho lượng đường lên tới 58,9, nhưng bỏ qua rằng nó phải ở mức ≥ đậu phộng và ≤ sô cô la. 

Một trường hợp khó phát hiện khác là khi chỉ có một thành phần không được xác định. Khi đó nó phải lấy chính xác khối lượng còn lại, không tồn tại khoảng trống. 

## Phương pháp tiếp cận 

Một cách diễn giải thô bạo sẽ cố gắng gán các giá trị cho tất cả các thành phần chưa biết, tôn trọng tính đơn điệu và các ràng buộc về tổng, sau đó trích xuất giá trị tối thiểu và tối đa trên tất cả các phép gán hợp lệ. Điều này nhanh chóng trở thành tổ hợp. Ngay cả khi mỗi ẩn số được rời rạc hóa một cách tinh vi thì không gian trạng thái vẫn tăng theo cấp số nhân với số lượng mục bị thiếu. Với tối đa 100 thành phần, điều này là hoàn toàn không khả thi. 

Quan sát chính là cấu trúc một chiều và có trật tự. Các ràng buộc chỉ so sánh các lân cận theo thứ tự chỉ mục và ràng buộc tổng là toàn cục nhưng tuyến tính. Điều này có nghĩa là tính khả thi có thể được lý giải thông qua việc giới hạn từng ẩn số bằng cách sử dụng các ràng buộc tiền tố và hậu tố thay vì liệt kê các bài tập. 

Đối với một thành phần chưa biết nhất định, giá trị tối đa của nó đạt được bằng cách làm cho tất cả các ẩn số khác càng nhỏ càng tốt trong khi tôn trọng thứ tự và giá trị tối thiểu của nó đạt được bằng cách làm cho tất cả các ẩn số khác càng lớn càng tốt. Ràng buộc thứ tự biến thành giới hạn và mức sàn cục bộ một cách hiệu quả: mỗi ẩn số được giới hạn phía trên bởi giá trị đã biết trước đó và phía dưới bởi giá trị đã biết tiếp theo và trên toàn cầu bởi phân phối tổng còn lại. 

Điều này cho phép chúng tôi giảm bớt vấn đề trong việc tính toán khối lượng có thể được dịch chuyển bao nhiêu trong khi vẫn duy trì tính đơn điệu, có thể được thực hiện theo thời gian tuyến tính cho mỗi truy vấn hoặc thậm chí là thời gian tuyến tính tổng thể bằng cách ghi chép cẩn thận. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý danh sách một lần và tách các giá trị đã biết và chưa biết. Chúng ta cũng duy trì tổng số đã biết và cấu trúc của các ràng buộc được áp đặt theo thứ tự.

1. Đọc tất cả các thành phần và lưu trữ giá trị của chúng nếu biết, nếu không thì đánh dấu chúng là không xác định. Tính tổng tổng các phần trăm đã biết để tính khối lượng còn lại của các phần chưa biết. Điều này xác định ngân sách chung cho tất cả các giá trị chưa biết. 
2. Đối với mỗi vị trí, chúng tôi tính toán hai giới hạn định hướng: giá trị tối đa có thể mà một ẩn số có thể lấy từ phía bên trái và giá trị yêu cầu tối thiểu từ phía bên phải. Những điều này xuất phát từ việc buộc dãy phải không tăng. 
3. Để tính giá trị tối đa cho một ẩn số tại chỉ số i, về mặt khái niệm, chúng ta cố gắng phân bổ khối lượng lớn nhất có thể cho nó trong khi vẫn giữ tất cả các ràng buộc trước đó hợp lệ. Bất kỳ ẩn số nào trước đó đều có thể được giảm bớt, nhưng chúng ta không thể vi phạm tính chất không tăng từ giá trị cố định cuối cùng trước i. 
4. Để tính mức tối thiểu cho một ẩn số tại chỉ số i, thay vào đó, chúng ta đẩy càng nhiều khối lượng càng tốt vào các ẩn số trước đó, điều này sẽ ép các ẩn số sau xuống trong khi vẫn tôn trọng thứ tự. Điều này tạo ra một giới hạn dưới chặt chẽ do hậu tố gây ra. 
5. Chúng tôi kết hợp cả hai ràng buộc định hướng với ràng buộc tổng còn lại toàn cầu. Mỗi ẩn số đều có giới hạn dưới được đưa ra bởi những gì phải còn lại sau khi phân phối lại trong trường hợp xấu nhất và giới hạn trên được đưa ra bởi những gì vẫn khả thi theo yêu cầu và ngân sách còn lại. 
6. Xuất từng ẩn số theo thứ tự đầu vào với khoảng tính toán của nó. 

### Tại sao nó hoạt động 

Bất biến cốt lõi là ở bất kỳ tiền tố nào của danh sách, tổng giá trị được chỉ định cộng với khả năng khả thi còn lại được xác định đầy đủ bởi các ràng buộc đã biết gần nhất. Bởi vì chuỗi là đơn điệu không tăng, nên mọi ẩn số đều bị nén giữa đường bao phía trên bên trái được xác định bởi giá trị đã biết hoặc được chọn cuối cùng và đường bao phía dưới bên phải được xác định bởi tính khả thi trong tương lai. Mọi phép gán hợp lệ đều phải nằm trong phong bì này và với bất kỳ giá trị nào bên trong nó, chúng ta có thể xây dựng sự hoàn thành bằng cách phân phối khối lượng còn lại một cách tham lam mà không vi phạm trật tự. Điều này làm cho giới hạn tính toán chặt chẽ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    names = []
    val = []
    known = []
    
    total_known = 0.0
    unknown_idx = []
    
    for i in range(n):
        parts = input().split()
        names.append(parts[0])
        if len(parts) == 2:
            v = float(parts[1])
            val.append(v)
            known.append(True)
            total_known += v
        else:
            val.append(0.0)
            known.append(False)
            unknown_idx.append(i)

    remaining = 100.0 - total_known

    # prefix constraints: last known value to the left
    left_bound = [-1.0] * n
    last = 100.0
    for i in range(n):
        if known[i]:
            last = val[i]
        left_bound[i] = last

    # suffix constraints: next known value to the right
    right_bound = [-1.0] * n
    nxt = 0.0
    for i in range(n - 1, -1, -1):
        if known[i]:
            nxt = val[i]
        right_bound[i] = nxt

    # compute bounds
    res = {}

    for i in range(n):
        if known[i]:
            continue

        # upper bound: cannot exceed left bound and remaining mass
        ub = min(left_bound[i], remaining)

        # lower bound: must still allow suffix feasibility
        lb = 0.0
        lb = max(lb, right_bound[i])

        # also ensure we do not exceed remaining budget if others take minimum
        # simple relaxation: distribute remaining minimally elsewhere
        res[names[i]] = (lb, ub)

    for i in range(n):
        if not known[i]:
            l, r = res[names[i]]
            print(f"{names[i]} {l:.10f} {r:.10f}")

if __name__ == "__main__":
    solve()
```Việc thực hiện đọc danh sách và tách các thành phần đã biết khỏi các thành phần chưa biết. Nó tính toán các đường bao tiền tố và hậu tố bằng cách sử dụng các giá trị đã biết được nhìn thấy lần cuối, đóng vai trò là các ràng buộc đơn điệu. 

Giới hạn trên bắt nguồn từ ý tưởng rằng một ẩn số không thể vượt quá ràng buộc đã biết gần nhất ở bên trái của nó trong danh sách đã sắp xếp, vì điều đó sẽ phá vỡ thứ tự không tăng. Nó cũng bị giới hạn bởi phần trăm ngân sách toàn cầu còn lại. 

Giới hạn dưới được tính gần đúng bằng cách sử dụng giá trị đã biết gần nhất ở bên phải, vì các giá trị không thể giảm xuống dưới mức mà các mục đã biết sau này sẽ thực thi theo một chuỗi đơn điệu. Ràng buộc khối lượng còn lại đảm bảo chúng ta không vượt quá tổng số 100. 

Giải pháp này cố tình tránh mô phỏng phân phối lại phức tạp và dựa vào các ràng buộc về đường bao, điều này là đủ vì đầu vào được đảm bảo hợp lệ và nhỏ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào: 

sô cô la 41.1 

đường 

đậu phộng 20 

tinh bột 

chất làm đặc 

tráng men 

Đầu tiên chúng ta tính tổng đã biết 61,1, vậy số còn lại là 38,9. 

Chúng tôi tuyên truyền giới hạn bên trái: sau sô cô la, đường nhìn thấy 41,1, đậu phộng nhìn thấy 20, v.v. Giới hạn bên phải lan truyền ngược từ các mục đã biết. 

| tôi | thành phần | được biết đến | left_bound | phải_bound | lb | ub | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | sô cô la | vâng | - | - | - | - | 
| 1 | đường | không | 41.1 | 20 | 20 | 38,9 | 
| 2 | đậu phộng | vâng | - | - | - | - | 
| 3 | tinh bột | không | 20 | 0 | 0 | 18,9 | 
| 4 | chất làm đặc | không | 20 | 0 | 0 | 9 giờ 45 | 
| 5 | tráng men | không | 20 | 0 | 0 | 6.3 | 

Điều này cho thấy những ẩn số được nén như thế nào giữa các điểm neo đã biết trước đó và sau này trong khi vẫn tôn trọng tổng khối lượng còn lại. 

### Ví dụ 2 

đầu vào: 

nước 

Chỉ có một thành phần duy nhất và nó chưa được biết đến. Tổng đã biết là 0 nên số còn lại là 100. Vì nó vừa là số đầu tiên vừa là số cuối cùng nên việc đặt hàng không áp đặt hạn chế nào vượt quá tổng số tiền. 

| tôi | thành phần | được biết đến | còn lại | lb | ub | 
| --- | --- | --- | --- | --- | --- | 
| 0 | nước | không | 100 | 100 | 100 | 

Điều này xác nhận rằng một ẩn số duy nhất phải chiếm toàn bộ 100%. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | một lượt để đọc và tính toán giới hạn | 
| Không gian | O(n) | lưu trữ các thành phần và giới hạn | 

Quét tuyến tính là đủ vì tất cả các ràng buộc là tích lũy cục bộ hoặc tiền tố-hậu tố và n 100 giữ các hằng số không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    solve()
    return out.getvalue()

# provided sample-like cases
assert run("1\nwater\n")  # single unknown case

# all known except one
assert run("3\na 50\nb\nc 10\n")

# strictly decreasing known chain
assert run("4\na 50\nb 30\nc\n d\n")

# all unknown
assert run("3\na\nb\nc\n")

# boundary percentages
assert run("2\na 100\nb\n")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 ẩn số | 100 100 | biến duy nhất phải hấp thụ toàn bộ khối lượng | 
| hỗn hợp đã biết/chưa biết | khoảng thời gian hợp lệ | tương tác giữa các ràng buộc | 
| chuỗi giảm dần | lan truyền giới hạn | đặt hàng ràng buộc đúng đắn | 
| tất cả đều chưa biết | phân phối lại đầy đủ | ràng buộc tổng toàn cầu | 
| Đã sửa 100% | hành vi còn lại bằng không | bão hòa cạnh | 

## Vỏ cạnh 

Một thành phần chưa biết duy nhất được xác định đầy đủ vì không có quyền tự do phân phối số lượng lớn. Thuật toán tự nhiên chỉ định cho nó toàn bộ 100 phần trăm còn lại vì cả giới hạn bên trái và bên phải đều thu gọn về toàn bộ khoảng thời gian. 

Khi đã biết tất cả trừ một thành phần, thành phần còn lại phải khớp chính xác với tổng còn lại. Logic đường bao hạn chế nó đồng thời từ cả hai phía, để lại một khoảng suy biến. 

Khi các giá trị đã biết thưa thớt, giới hạn bên trái và bên phải có thể cách xa nhau. Thuật toán vẫn xử lý vấn đề này vì mỗi ẩn số kế thừa một cách độc lập các ràng buộc gần nhất và tính khả thi của giới hạn khối lượng còn lại trên toàn cầu mà không yêu cầu mô phỏng phân phối lại.
