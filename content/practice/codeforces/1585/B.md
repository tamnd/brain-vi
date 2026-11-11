---
title: "CF 1585B - Đảo ngược mảng"
description: "Chúng ta được cho một dãy số và liên tục áp dụng một phép biến đổi chỉ phụ thuộc vào phần tử cuối cùng của dãy hiện tại."
date: "2026-06-10T09:28:11+07:00"
tags: ["codeforces", "competitive-programming", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1585
codeforces_index: "B"
codeforces_contest_name: "Technocup 2022 - Elimination Round 3"
rating: 900
weight: 1585
solve_time_s: 108
verified: true
draft: false
---

[CF 1585B - Đảo ngược mảng](https://codeforces.com/problemset/problem/1585/B) 

**Xếp hạng:** 900 
**Thẻ:** tham lam 
**Thời gian giải:** 1 phút 48s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một dãy số và liên tục áp dụng một phép biến đổi chỉ phụ thuộc vào phần tử cuối cùng của dãy hiện tại. Mỗi phép biến đổi chia mảng thành hai chuỗi con: các phần tử không lớn hơn phần tử cuối cùng sẽ ở phía trước và các phần tử lớn hơn sẽ ở phía sau, trong khi vẫn giữ nguyên thứ tự tương đối ban đầu của chúng trong mỗi nhóm. Mảng sau đó được thay thế bằng phép nối này. 

Chúng tôi được hỏi chúng tôi cần áp dụng thao tác này bao nhiêu lần cho đến khi mảng ngừng thay đổi. Sau khi áp dụng phép biến đổi không còn làm thay đổi mảng nữa, các ứng dụng tiếp theo sẽ giữ nguyên mảng đó và chúng tôi xuất ra khoảnh khắc đầu tiên quá trình ổn định này xảy ra. 

Các ràng buộc cho phép tổng cộng tối đa 2·10^5 phần tử trong các trường hợp thử nghiệm, điều này ngay lập tức loại trừ mọi mô phỏng bậc hai trong trường hợp xấu nhất. Một cách tiếp cận đơn giản là liên tục xây dựng lại mảng và quét toàn bộ chuỗi cho mỗi thao tác có thể giảm xuống O(n^2) khi mảng phát triển chậm, tức là quá chậm so với giới hạn. 

Trường hợp cạnh tinh tế xuất hiện khi mảng đã ở một điểm cố định. Ví dụ: nếu tất cả các phần tử đều bằng nhau, chẳng hạn [7, 7, 7, 7], thì phần tử cuối cùng sẽ phân chia mọi thứ sang bên trái và không có gì di chuyển sang bên phải, do đó mảng không bao giờ thay đổi. Câu trả lời đúng là 0. Một mô phỏng bất cẩn vẫn có thể thực hiện một lần lặp đầy đủ và tính không chính xác đó là một thay đổi có ý nghĩa. 

Một trường hợp quan trọng khác là khi mảng tăng nghiêm ngặt. Đối với [1, 2, 3, 4], phần tử cuối cùng luôn là giá trị lớn nhất nên một lần nữa không có gì di chuyển và câu trả lời là 0. Điều này cho thấy quá trình không nhất thiết phải “sắp xếp”, nó chỉ tổ chức lại khi phần tử lớn hơn xuất hiện trước trục xoay. 

## Phương pháp tiếp cận 

Mô phỏng trực tiếp rất đơn giản: đối với mỗi bước, hãy lấy phần tử cuối cùng, quét mảng, chia nó thành hai danh sách tùy thuộc vào việc các phần tử có nằm trong trục này hay không và nối chúng lại. Mỗi bước tốn O(n). Vấn đề là chúng ta có thể cần mô phỏng tối đa O(n) bước trong trường hợp xấu nhất, vì mỗi thao tác chỉ cải thiện một phần thứ tự. Điều này dẫn đến O(n^2), quá lớn đối với tổng số 2·10^5 phần tử. 

Quan sát quan trọng là quá trình này được điều khiển bởi vị trí của các phần tử lớn hơn trục xoay hiện tại. Mỗi thao tác đều “đẩy về phía trước” một cách hiệu quả ranh giới giữa các phần tử đã được đặt chính xác so với cấu trúc được sắp xếp cuối cùng. Thay vì mô phỏng các thay đổi, chúng ta có thể suy luận xem có bao nhiêu phần tử đã có trong một hậu tố ổn định trong mọi hoạt động trong tương lai. 

Nếu chúng tôi quét từ cuối, chúng tôi có thể duy trì giá trị tối đa được thấy cho đến nay. Bất cứ khi nào chúng tôi gặp phải một phần tử lớn hơn mức tối đa đang chạy này, điều đó có nghĩa là phần tử này cuối cùng sẽ buộc thêm ít nhất một lần chuyển đổi không tầm thường nữa trước khi nó ổn định vào vùng chính xác. Mỗi “sự phá vỡ sự thống trị của hậu tố” như vậy tương ứng với một thao tác bắt buộc. 

Cụ thể, chúng tôi đếm số lần chúng tôi nhìn thấy mức tối đa mới khi di chuyển từ phải sang trái, ngoại trừ chính phần tử tối đa cuối cùng. Số lượng này tương ứng trực tiếp với số lượng thay đổi hiệu quả trước khi ổn định. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(n^2) | O(n) | Quá chậm | 
| Quét ngược với tính năng theo dõi tối đa | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Bắt đầu từ phần tử cuối cùng của mảng và coi nó là phần tử tối đa hiện tại. Phần tử này sẽ không bao giờ di chuyển qua chính nó trong bất kỳ thao tác nào, vì vậy nó tạo thành hậu tố ổn định ban đầu. 
2. Di chuyển mảng từ phải sang trái, duy trì giá trị lớn nhất cho đến nay theo hướng này. Mức tối đa này thể hiện ngưỡng xác định xem các phần tử trước đó đã “tương thích” với thứ tự cuối cùng hay chưa. 
3. Mỗi lần chúng tôi gặp một phần tử lớn hơn mức tối đa hiện tại, chúng tôi sẽ cập nhật mức tối đa và tăng bộ đếm. Sự kiện này thể hiện sự vi phạm cấu trúc trong thứ tự hậu tố không thể giải quyết được nếu không có ít nhất một bước chuyển đổi. 
4. Sau khi kết thúc quá trình truyền tải, bộ đếm biểu thị số lần cấu trúc của mảng phải được sửa chữa trước khi đạt đến điểm cố định. 
5. Trả lại bộ đếm này làm câu trả lời. 

### Tại sao nó hoạt động 

Mỗi phiên bản sử dụng phần tử cuối cùng làm trục và phân tách các phần tử dựa trên giá trị đó. Cấu hình ổn định cuối cùng tương ứng với trường hợp khi nhìn từ bên phải, không có phần tử nào trước đó vượt quá phần tử hậu tố tối đa. Mỗi khi một mức tối đa mới xuất hiện trong khi quét từ bên phải, nó sẽ đánh dấu một điểm mà cuối cùng phải trở thành trục của một số thao tác trước khi mảng có thể ổn định. Vì các giá trị cực đại này xuất hiện theo thứ tự ngược lại với cách chúng được giải quyết bằng các phép biến đổi, nên việc đếm chúng sẽ đưa ra chính xác số lượng thao tác cần thiết cho đến khi không thể sắp xếp lại được nữa. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        
        mx = 0
        ans = 0
        
        for i in range(n - 1, -1, -1):
            if a[i] > mx:
                mx = a[i]
                ans += 1
        
        out.append(str(max(0, ans - 1)))
    
    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Mã xử lý từng trường hợp thử nghiệm một cách độc lập. Quá trình quét ngược duy trì mức tối đa đang chạy. Mỗi khi một mức tối đa mới xuất hiện, nó sẽ tăng một bộ đếm. Câu trả lời cuối cùng được điều chỉnh bằng cách trừ đi một vì mức tối đa nhìn thấy lần cuối tương ứng với hậu tố đã ổn định và không yêu cầu thao tác. Phép trừ tinh tế này là chi tiết triển khai chính đảm bảo tính chính xác: nếu không có nó, các mảng được sắp xếp đầy đủ sẽ trả về 1 thay vì 0 không chính xác. 

## Ví dụ đã hoạt động 

### Ví dụ 1: [2, 4, 1, 5, 3] 

Chúng tôi theo dõi hậu tố cực đại từ phải sang trái. 

| Chỉ mục (phải→trái) | Giá trị | Tối đa hiện tại | Tối đa mới? | Quầy | 
| --- | --- | --- | --- | --- | 
| 4 | 3 | 3 | vâng | 1 | 
| 3 | 5 | 5 | vâng | 2 | 
| 2 | 1 | 5 | không | 2 | 
| 1 | 4 | 5 | không | 2 | 
| 0 | 2 | 5 | không | 2 | 

Chúng ta nhận được bộ đếm = 2 và sau khi trừ 1, chúng ta nhận được 1. Điều này phù hợp với thực tế là sau một lần đảo ngược có ý nghĩa, mảng sẽ trở nên ổn định. 

### Ví dụ 2: [5, 3, 2, 4, 1] 

| Chỉ mục (phải→trái) | Giá trị | Tối đa hiện tại | Tối đa mới? | Quầy | 
| --- | --- | --- | --- | --- | 
| 4 | 1 | 1 | vâng | 1 | 
| 3 | 4 | 4 | vâng | 2 | 
| 2 | 2 | 4 | không | 2 | 
| 1 | 3 | 4 | không | 2 | 
| 0 | 5 | 5 | vâng | 3 | 

Bộ đếm = 3, câu trả lời cuối cùng = 2. Điều này phù hợp với quy trình ổn định hai bước được trình bày trong câu lệnh. 

Những dấu vết này cho thấy rằng mỗi lần một phần tử lớn hơn xuất hiện ở bên trái của hậu tố tối đa hiện tại, nó tương ứng với một sự điều chỉnh cấu trúc cần thiết trong chuỗi các phiên bản. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi phần tử được truy cập một lần cho mỗi trường hợp thử nghiệm trong một lần đảo ngược duy nhất | 
| Không gian | O(1) | Chỉ có một số biến vô hướng được duy trì | 

Giải pháp dễ dàng phù hợp với các ràng buộc vì tổng số phần tử trong tất cả các trường hợp thử nghiệm là 2·10^5, giúp việc quét tuyến tính trên mỗi trường hợp thử nghiệm trở nên hiệu quả. 

## Trường hợp thử nghiệm```python
import sys, io

def solve():
    input = sys.stdin.readline
    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        
        mx = 0
        ans = 0
        for i in range(n - 1, -1, -1):
            if a[i] > mx:
                mx = a[i]
                ans += 1
        
        out.append(str(max(0, ans - 1)))
    
    print("\n".join(out))

def run(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    import io as sio
    out = sio.StringIO()
    with redirect_stdout(out):
        solve()
    sys.stdin = old_stdin
    return out.getvalue().strip()

# provided samples
assert run("3\n5\n2 4 1 5 3\n5\n5 3 2 4 1\n4\n1 1 1 1\n") == "1\n2\n0"

# all equal
assert run("1\n4\n7 7 7 7\n") == "0"

# increasing
assert run("1\n5\n1 2 3 4 5\n") == "0"

# decreasing
assert run("1\n5\n5 4 3 2 1\n") == "4"

# single element
assert run("1\n1\n42\n") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả các mảng bằng nhau | 0 | ổn định ngay lập tức | 
| mảng tăng dần | 0 | cấu trúc hậu tố đã ổn định | 
| mảng giảm dần | 4 | số lần điều chỉnh trong trường hợp xấu nhất | 
| phần tử đơn | 0 | điều kiện biên |
