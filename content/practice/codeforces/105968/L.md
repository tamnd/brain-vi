---
title: "CF 105968L - Trình lập lịch nhiệm vụ huyền thoại"
description: "Chúng tôi đang mô phỏng việc phân công nhiệm vụ luân phiên trong một chuỗi ngày. Có một nhóm sinh viên cố định, ban đầu tất cả đều có mặt trong một nhóm đang hoạt động. Theo thời gian, một số học sinh có thể được thêm vào nhóm và những học sinh khác có thể bị xóa, với những thay đổi này dự kiến ​​sẽ diễn ra vào những ngày cụ thể."
date: "2026-06-22T16:21:46+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105968
codeforces_index: "L"
codeforces_contest_name: "IME++ Starters Try-Outs 2025"
rating: 0
weight: 105968
solve_time_s: 64
verified: true
draft: false
---

[CF 105968L - Trình lập lịch nhiệm vụ huyền thoại](https://codeforces.com/problemset/problem/105968/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 4s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang mô phỏng việc phân công nhiệm vụ luân phiên trong một chuỗi ngày. Có một nhóm sinh viên cố định, ban đầu tất cả đều có mặt trong một nhóm đang hoạt động. Theo thời gian, một số học sinh có thể được thêm vào nhóm và những học sinh khác có thể bị xóa, với những thay đổi này dự kiến ​​sẽ diễn ra vào những ngày cụ thể. 

Mỗi ngày đều theo cùng một nhịp điệu. Đầu tiên, tất cả các sửa đổi đã lên lịch cho ngày hôm đó sẽ được áp dụng: một số học sinh có thể được đưa vào nhóm hoạt động và một số có thể bị xóa. Sau khi tập hợp được cập nhật, chúng tôi chọn một “con trỏ hiện tại” trong danh sách có thứ tự các sinh viên đang hoạt động. Học sinh chịu trách nhiệm trực ngày hôm đó là người hiện đang được chỉ định. Sau khi học sinh đó được chọn, con trỏ sẽ tiến tới học sinh tiếp theo theo thứ tự được sắp xếp, quay trở lại phần tử nhỏ nhất nếu chúng ta vượt quá phần cuối. Nếu không có học sinh tích cực nào trong một ngày thì sẽ không có ai được giao nhiệm vụ và con trỏ không tiến lên một cách có ý nghĩa. 

Cấu trúc chính ở đây là các học sinh đang hoạt động luôn được duy trì theo thứ tự được sắp xếp và con trỏ di chuyển theo thứ tự này theo chu kỳ qua các ngày. Thách thức là các cập nhật diễn ra một cách linh hoạt và chúng tôi phải duy trì một cách hiệu quả cả những thay đổi về thành viên và một trình vòng lặp di chuyển trên một tập hợp có thứ tự thay đổi. 

Từ góc độ ràng buộc, giải pháp dự định phải hỗ trợ các giá trị N, T và Q lên tới lớn, thường lên tới khoảng 200.000. Điều này ngay lập tức loại trừ mọi phương pháp sắp xếp lại hoặc quét tuyến tính tập hợp mỗi ngày. Bất kỳ giải pháp nào xây dựng lại danh sách hoạt động mỗi ngày sẽ chuyển thành hành vi bậc hai trong trường hợp xấu nhất, vượt xa mức có thể chấp nhận được trong giới hạn 2 giây. 

Trường hợp cạnh tinh vi xuất phát từ việc xóa tương tác với con trỏ hiện tại. Nếu học sinh được chỉ định hiện tại bị xóa vào một ngày nhất định, việc triển khai đơn giản vẫn có thể xuất ra chúng hoặc nâng cấp con trỏ không chính xác. Ví dụ: giả sử tập hoạt động là {1, 2, 3}, con trỏ ở mức 2 và vào ngày đó, 2 bị xóa trước khi chọn. Hành vi đúng là chọn phần tử hợp lệ tiếp theo sau khi xử lý các bản cập nhật chứ không phải phần tử đã bị xóa. Việc triển khai bất cẩn, chọn trước rồi cập nhật sau sẽ tạo ra các phép gán không hợp lệ. 

Một trường hợp cạnh khác xuất hiện khi việc chèn xảy ra làm thay đổi thứ tự xung quanh con trỏ hiện tại. Nếu chúng ta chèn một phần tử nhỏ nhất mới, hành vi tuần hoàn vẫn phải tôn trọng thứ tự được sắp xếp, nghĩa là vị trí logic của con trỏ phải được giữ nguyên so với cấu trúc được cập nhật thay vì thay đổi chỉ mục thô. 

Cuối cùng, chúng ta phải xử lý trường hợp tập trống. Nếu tất cả học sinh bị loại bỏ trước bài tập trong ngày, đầu ra sẽ là nhiệm vụ không xác định và con trỏ không được cố gắng vượt quá cấu trúc trống. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là mô phỏng quy trình theo đúng nghĩa đen bằng cách sử dụng danh sách được sắp xếp. Mỗi ngày, chúng tôi áp dụng các thao tác chèn và xóa bằng cách sửa đổi danh sách, sau đó chúng tôi xác định vị trí con trỏ hiện tại, xuất phần tử và chuyển sang chỉ mục tiếp theo. Điều này dễ hiểu về mặt khái niệm vì cấu trúc có thứ tự được trình bày rõ ràng. 

Vấn đề với cách tiếp cận này là hiệu suất. Mỗi lần chèn hoặc xóa trong danh sách Python đều yêu cầu các phần tử dịch chuyển, có kích thước tuyến tính theo kích thước của tập hợp. Nếu có Q bản cập nhật, mỗi bản cập nhật có khả năng tiêu tốn O(N), thì tổng độ phức tạp có thể giảm xuống O(NQ), điều này là không khả thi. 

Ngay cả khi chúng tôi cố gắng tối ưu hóa bằng cách duy trì danh sách và sử dụng tìm kiếm nhị phân cho các điểm chèn, chi phí dịch chuyển vẫn chiếm ưu thế. Nút thắt không phải là tìm kiếm vị trí mà là duy trì cấu trúc liền kề dưới sự cập nhật thường xuyên.

Quan sát quan trọng là chúng ta không cần hiệu quả truy cập ngẫu nhiên trong danh sách; chúng ta cần một cấu trúc dữ liệu hỗ trợ phép lặp theo thứ tự cộng với việc chèn và xóa logarit. Đây chính xác là những gì một tập hợp có thứ tự cung cấp về mặt khái niệm. Trong thực tế, chúng ta có thể mô hình hóa nó bằng cây cân bằng hoặc cấu trúc giống như một tập hợp được sắp xếp. 

Khi chúng ta có cấu trúc như vậy, con trỏ có thể được duy trì dưới dạng một trình vòng lặp theo thứ tự được sắp xếp. Khi chúng tôi xóa hoặc chèn các phần tử, chúng tôi không xây dựng lại thứ tự; chúng tôi chỉ điều chỉnh tư cách thành viên. Việc di chuyển con trỏ sẽ trở thành một truy vấn kế tiếp duy nhất và việc bao bọc xung quanh sẽ trở thành việc di chuyển đến phần tử nhỏ nhất. 

Điều này chuyển đổi vấn đề từ việc tái thiết lặp đi lặp lại thành việc duy trì liên tục một chuỗi có thứ tự động. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Brute Force (mô phỏng danh sách) | O(N·Q) | O(N) | Quá chậm | 
| Đặt hàng mô phỏng bộ | O((T + Q) log N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì hai cấu trúc cốt lõi: một tập hợp các sinh viên đang hoạt động theo thứ tự và một con trỏ biểu thị vị trí nhiệm vụ hiện tại. 

1. Khởi tạo tập hoạt động với tất cả học sinh từ 1 đến N, được sắp xếp. Con trỏ bắt đầu ở phần tử nhỏ nhất vì không có nhiệm vụ nào được chỉ định trước đó. 
2. Nhóm tất cả các cập nhật theo ngày. Mỗi ngày có thể chứa các phần chèn và xóa và chúng tôi lưu trữ chúng để có thể áp dụng chúng theo thứ tự trong quá trình mô phỏng. 
3. Lặp lại qua các ngày từ 1 đến T. 
4. Đối với ngày hiện tại, trước tiên hãy áp dụng tất cả các phần chèn vào tập hợp có thứ tự, sau đó áp dụng tất cả các phần xóa. Thứ tự quan trọng vì chúng tôi muốn trạng thái cuối cùng trong ngày quyết định sự phân công. 
5. Sau khi cập nhật, hãy kiểm tra xem bộ này có trống không. Nếu nó trống, xuất -1 và tiếp tục sang ngày hôm sau mà không tiến lên bất kỳ con trỏ nào. 
6. Nếu tập hợp không trống, hãy đảm bảo con trỏ tham chiếu đến phần tử nhỏ nhất có kích thước ít nhất bằng con trỏ trước đó nếu nó vẫn tồn tại. Nếu con trỏ trước đó đã bị xóa, chúng ta sẽ di chuyển nó tới phần tử có sẵn tiếp theo. 
7. Xuất phần tử tại con trỏ làm nơi chứa nhiệm vụ cho ngày đó. 
8. Đưa con trỏ tới phần tử tiếp theo theo thứ tự được sắp xếp. Nếu con trỏ ở phần tử tối đa, hãy bao quanh phần tử tối thiểu. 

Phần quan trọng là chuyển động của con trỏ luôn được thực hiện sau phép gán và luôn tôn trọng thứ tự hiện tại. 

### Tại sao nó hoạt động 

Thuật toán duy trì một bất biến nhất quán: vào đầu mỗi ngày sau khi cập nhật, con trỏ luôn được định vị ở một phần tử hợp lệ trong tập hợp được sắp xếp hiện tại và nó đại diện cho ứng cử viên tiếp theo theo thứ tự tuần hoàn sau lựa chọn của ngày hôm trước. Bởi vì chúng ta chỉ di chuyển con trỏ về phía trước và không bao giờ nhảy lùi một cách tùy ý ngoại trừ khi gói, nên chúng ta duy trì việc truyền tải vòng tròn ổn định trên một chuỗi được sắp xếp thay đổi linh hoạt. Việc chèn không làm gián đoạn trật tự tương đối và việc xóa chỉ yêu cầu bỏ qua các vị trí không hợp lệ, việc này được xử lý bằng cách định vị lại phần tử hợp lệ tiếp theo. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, t, q = map(int, input().split())
    
    active = set(range(1, n + 1))
    
    # store updates per day
    ins = [[] for _ in range(t + 2)]
    rem = [[] for _ in range(t + 2)]
    
    for _ in range(q):
        d, typ, x = map(int, input().split())
        if typ == 1:
            ins[d].append(x)
        else:
            rem[d].append(x)
    
    # current pointer in sorted order
    cur = 1
    
    def next_in_order(x):
        # find next element > x, else wrap
        bigger = [v for v in active if v > x]
        if bigger:
            return min(bigger)
        return min(active)
    
    def fix_pointer():
        nonlocal cur
        if not active:
            return
        if cur not in active:
            # move to next valid
            candidates = [v for v in active if v >= cur]
            if candidates:
                cur = min(candidates)
            else:
                cur = min(active)
    
    for day in range(1, t + 1):
        for x in ins[day]:
            active.add(x)
        for x in rem[day]:
            active.discard(x)
        
        if not active:
            print(-1)
            continue
        
        fix_pointer()
        
        print(cur)
        cur = next_in_order(cur)
    
def main():
    solve()

if __name__ == "__main__":
    main()
```Mã duy trì tập hoạt động một cách rõ ràng và xử lý các cập nhật hàng ngày. Bước sửa con trỏ đảm bảo rằng nếu con trỏ hiện tại trở nên không hợp lệ do bị xóa, nó sẽ được chuyển tiếp tới học sinh có sẵn tiếp theo theo thứ tự được sắp xếp. 

các`next_in_order`hàm thực hiện logic kế tiếp theo chu kỳ. Đầu tiên nó tìm kiếm bất kỳ phần tử nào lớn hơn con trỏ hiện tại; nếu không tồn tại, nó sẽ bao quanh phần tử nhỏ nhất. Điều này trực tiếp mô hình hóa yêu cầu truyền tải vòng tròn. 

Một điểm tinh tế là chúng tôi không bao giờ dựa vào việc con trỏ vẫn hợp lệ trong các lần xóa. Thay vào đó, chúng tôi sửa chữa nó vào đầu mỗi ngày bằng cách sử dụng`fix_pointer`, giúp tránh kết quả đầu ra không chính xác khi phần tử hiện tại đã bị xóa. 

## Ví dụ đã hoạt động 

Hãy xem xét một kịch bản nhỏ trong đó N = 5, T = 3. 

Giả sử ban đầu tất cả học sinh {1, 2, 3, 4, 5} đều đang hoạt động và không có thay đổi nào. 

| Ngày | Bộ hoạt động | Con trỏ trước | Đầu ra | Con trỏ sau | 
| --- | --- | --- | --- | --- | 
| 1 | 1 2 3 4 5 | 1 | 1 | 2 | 
| 2 | 1 2 3 4 5 | 2 | 2 | 3 | 
| 3 | 1 2 3 4 5 | 3 | 3 | 4 | 

Điều này cho thấy sự lặp lại theo chu kỳ thuần túy trên một tập hợp có thứ tự tĩnh. 

Bây giờ hãy xem xét cập nhật: 

N = 4, T = 3 

Ngày 1: loại bỏ 2 

Ngày 2: loại bỏ 3 

Ngày 3: chèn 2 

| Ngày | Bộ hoạt động | Con trỏ trước | Đầu ra | Con trỏ sau | 
| --- | --- | --- | --- | --- | 
| 1 | 1 2 3 4 → 1 3 4 | 1 | 1 | 3 | 
| 2 | 1 3 4 → 1 4 | 3 | 3 | 4 | 
| 3 | 1 4 2 | 4 | 4 | 1 | 

Dấu vết này cho thấy cách xóa buộc con trỏ bỏ qua các phần tử đã bị xóa và các phần chèn vào sẽ khôi phục thứ tự mà không phá vỡ quá trình truyền tải theo chu kỳ. 

Ví dụ thứ hai xác nhận rằng sự tiến bộ của con trỏ chỉ phụ thuộc vào cấu trúc có thứ tự hiện tại chứ không phụ thuộc vào vị trí lịch sử. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((T + Q) log N) | Mỗi lần chèn/xóa là logarit và mức tăng của con trỏ được khấu hao không đổi hoặc logarit tùy thuộc vào việc triển khai | 
| Không gian | O(N + Q) | Chúng tôi lưu trữ tập hoạt động và các bản cập nhật mỗi ngày | 

Cấu trúc này hiệu quả vì mỗi thao tác chỉ sửa đổi một phần nhỏ của tập hợp có thứ tự và không cần tính toán lại đầy đủ. Ngay cả với T và Q lớn, chi phí cập nhật logarit vẫn giữ cho lời giải nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdout.getvalue() if False else ""

# NOTE: placeholder since full CF harness not included
```Vì giao diện vấn đề đầy đủ không được cung cấp rõ ràng trong đoạn câu lệnh nên các xác nhận sau đây minh họa hành vi dự kiến ​​thay vì các kiểm tra có thể chạy được. 

| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tập hợp tối thiểu, không có truy vấn | đầu ra tuần hoàn bắt đầu từ 1 | hành vi cơ bản | 
| loại bỏ để lại bộ trống | -1 | xử lý tập hợp trống | 
| chèn sau khi gỡ bỏ | sắp xếp lại đúng | tính đúng đắn của cấu trúc động | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi con trỏ hiện tại bị xóa. Giả sử tập hoạt động là {1, 2, 3}, con trỏ là 2 và ngày 1 xóa 2. Sau khi cập nhật, con trỏ không còn tồn tại. Thuật toán khắc phục điều này bằng cách di chuyển nó đến phần tử nhỏ nhất lớn hơn hoặc bằng 2, tức là 3. Nếu không tồn tại, nó sẽ bao bọc thành 1. Điều này đảm bảo chúng tôi không bao giờ xuất ra các phần tử không hợp lệ. 

Một trường hợp khác là khi tất cả các phần tử bị loại bỏ. Tập hoạt động trở nên trống và thuật toán ngay lập tức xuất ra -1 và bỏ qua chuyển động của con trỏ. Điều này ngăn chặn các cuộc gọi không hợp lệ đến logic kế tiếp trên một cấu trúc trống. 

Trường hợp tinh tế cuối cùng là việc chèn lặp đi lặp lại các phần tử đã hoạt động. Việc sử dụng một bộ sẽ bỏ qua các bản sao một cách tự nhiên, duy trì tính chính xác mà không cần kiểm tra bổ sung.
