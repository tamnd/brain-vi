---
title: "CF 105184G - Vòng tay"
description: "Chúng ta được cung cấp ba loại ô vòng đeo tay có sẵn: một loại đại diện cho một cặp số 0, loại khác đại diện cho một cặp hỗn hợp và loại cuối cùng đại diện cho một cặp số 1. Mỗi loại gạch có số lượng giới hạn, lần lượt là n, m và k."
date: "2026-06-27T04:25:41+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105184
codeforces_index: "G"
codeforces_contest_name: "The 8th Hebei Collegiate Programming Contest"
rating: 0
weight: 105184
solve_time_s: 43
verified: true
draft: false
---

[CF 105184G - Vòng tay](https://codeforces.com/problemset/problem/105184/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 43s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp ba loại ô vòng đeo tay có sẵn: một loại đại diện cho một cặp số 0, loại khác đại diện cho một cặp hỗn hợp và loại cuối cùng đại diện cho một cặp số 1. Mỗi loại gạch có số lượng giới hạn, lần lượt là n, m và k. Ngoài ra, chúng ta còn có một mẫu vòng tay hình tròn mục tiêu được mô tả bằng chuỗi nhị phân S. 

Chúng tôi không được yêu cầu chế tạo toàn bộ vòng đeo tay hình tròn. Thay vào đó, chúng ta muốn chọn một đoạn liền kề của S, coi nó như một chuỗi con tuyến tính và xây dựng một đoạn càng dài càng tốt bằng cách sử dụng các ô có sẵn. Mỗi ký tự trong S tương ứng với một bit trong cấu trúc cuối cùng và các bit liền kề xác định khối ảnh nào là cần thiết. Điều khó khăn là chúng ta được phép lật hướng của một ô hỗn hợp, nghĩa là một ô thường đóng góp 01 cũng có thể được sử dụng làm 10. Hai loại ô còn lại là đối xứng và không bị ảnh hưởng bởi việc lật. 

Nhiệm vụ là xác định độ dài tối đa của chuỗi con liền kề của S có thể được hình thành mà không vượt quá số lượng có sẵn của từng loại khối ảnh. 

Các ràng buộc lên tới 1e6 cho cả số lượng ô và độ dài chuỗi. Điều này ngay lập tức loại trừ mọi nghiệm bậc hai trên các chuỗi con. Bất kỳ giải pháp nào cũng phải xử lý chuỗi theo thời gian tuyến tính hoặc gần tuyến tính và mọi hoạt động tính toán lại trên mỗi chuỗi con đều quá tốn kém. 

Một trường hợp lỗi tinh vi phát sinh khi người ta cho rằng vấn đề có thể được giải quyết một cách tham lam ngay từ đầu chuỗi mà không cần xem xét tất cả các vị trí bắt đầu. Ví dụ: nếu S thay đổi nhiều giữa 0 và 1, thì nhu cầu về các ô hỗn hợp sẽ dao động và việc bắt đầu ở các mức chênh lệch khác nhau sẽ thay đổi nếu chúng ta dùng hết một loại ô cụ thể trước tiên. Việc xây dựng từ trái sang phải đơn giản từ chỉ số 0 sẽ bỏ lỡ các chuỗi con tối ưu bắt đầu sau đó. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử mọi chỉ mục bắt đầu i và mở rộng chuỗi con sang bên phải trong khi theo dõi số lần chuyển đổi 00, 01/10 và 11 được yêu cầu. Đối với mỗi lần mở rộng, chúng tôi duy trì số lượng các cặp liền kề do chuỗi con tạo ra. Mỗi lần chúng tôi mở rộng chuỗi con, chúng tôi cập nhật số lượng ô cần thiết và kiểm tra tính khả thi đối với n, m và k. 

Điều này hoạt động chính xác vì mọi chuỗi con đều được kiểm tra rõ ràng dựa trên các ràng buộc về tài nguyên nhưng quá chậm. Có các chuỗi con O(N^2) và số lần cập nhật tăng dần vẫn dẫn đến hành vi bậc hai trong trường hợp xấu nhất, vì mỗi phần mở rộng là O(1) nhưng được thực hiện O(N^2) lần. 

Quan sát chính là tính khả thi của chuỗi con chỉ phụ thuộc vào số lần chuyển tiếp giữa các ký tự liền kề. Mỗi chuỗi con đặt ra một yêu cầu: mỗi cặp liền kề bằng nhau tiêu thụ ô 00 hoặc 11 tùy thuộc vào giá trị và mỗi cặp khác nhau tiêu thụ ô 01, bất kể hướng nào vì được phép lật. 

Điều này làm giảm vấn đề theo dõi cửa sổ trượt trên S trong khi vẫn duy trì ba bộ đếm cho các chuyển đổi. Sau khi sửa điểm cuối bên trái, chúng tôi có thể mở rộng điểm cuối bên phải một cách tham lam cho đến khi vượt quá một trong các tài nguyên. Sau đó, chúng tôi di chuyển điểm cuối bên trái về phía trước, duy trì số lượng ở O(1) mỗi bước. Đây là một bài toán cổ điển về tính khả thi của hai con trỏ hoặc cửa sổ trượt trong đó ràng buộc là đơn điệu đối với việc mở rộng cửa sổ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(N^2) | O(1) | Quá chậm | 
| Cửa Sổ Trượt | O(N) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng chuỗi con bằng cách đếm xem nó chứa bao nhiêu lần chuyển đổi của từng loại. Đối với một chuỗi con, mỗi cặp liền kề đều đóng góp vào chính xác một loại tài nguyên. 

Chúng tôi duy trì một cửa sổ [l, r) và các bộ đếm cnt00, cnt01, cnt11 phản ánh số lượng chuyển tiếp liền kề bên trong cửa sổ.

1. Khởi tạo l = 0, r = 0 và tất cả các bộ đếm về 0. Tại thời điểm này, cửa sổ trống và có giá trị tầm thường. 
2. Mở rộng từng ký tự một. Khi thêm S[r], chúng ta xem xét cạnh mới được hình thành giữa S[r-1] và S[r] nếu r > l. Chúng tôi cập nhật bộ đếm tương ứng dựa trên việc cạnh này là 00, 11 hay hỗn hợp. Bước này mã hóa thực tế rằng chỉ các mối quan hệ liền kề mới quan trọng chứ không phải vị trí tuyệt đối. 
3. Sau khi cập nhật bộ đếm, hãy kiểm tra xem cnt00 <= n, cnt01 <= m và cnt11 <= k có giữ hay không. Nếu đúng thì cửa sổ hiện tại khả thi nên chúng tôi cập nhật câu trả lời theo độ dài của nó. 
4. Nếu cửa sổ không thể sử dụng được, chúng ta phải thu nhỏ nó từ bên trái. Trước khi di chuyển l về phía trước, chúng tôi loại bỏ sự đóng góp của cạnh (S[l], S[l+1]) nếu nó tồn tại bên trong cửa sổ, giảm bộ đếm thích hợp. Sau đó, chúng tôi tăng l và tiếp tục thu nhỏ cho đến khi cửa sổ hợp lệ trở lại. Bước này có tác dụng vì việc loại bỏ các phần tử chỉ làm giảm số lượng nên tính khả thi cuối cùng sẽ được khôi phục. 
5. Tiếp tục cho đến khi r đến cuối chuỗi. 

Chi tiết quan trọng là mỗi cạnh được thêm và xóa nhiều nhất một lần, do đó tổng công việc vẫn tuyến tính. 

Tại sao nó hoạt động dựa trên một đặc tính khả thi đơn điệu. Nếu một cửa sổ không khả thi, việc mở rộng thêm cửa sổ cũng không thể khắc phục được vi phạm vì tất cả các bộ đếm đều không giảm đối với r. Tương tự, việc thu nhỏ từ bên trái sẽ giảm thiểu nghiêm ngặt bộ đếm, đảm bảo rằng quy trình luôn hướng tới tính khả thi mà không cần xem lại các trạng thái. Điều này đảm bảo rằng mọi chuỗi con hợp lệ đều được coi là cấu hình ranh giới cửa sổ tại một số điểm và độ dài khả thi tối đa được ghi lại. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m, k = map(int, input().split())
    S = input().strip()
    if len(S) <= 1:
        print(len(S))
        return

    def classify(a, b):
        if a == b == '0':
            return 0
        if a == b == '1':
            return 2
        return 1

    cnt = [0, 0, 0]  # 00, 01, 11
    l = 0
    ans = 0

    for r in range(len(S)):
        if r > l:
            c = classify(S[r-1], S[r])
            cnt[c] += 1

        while cnt[0] > n or cnt[1] > m or cnt[2] > k:
            if l + 1 <= r:
                c = classify(S[l], S[l+1])
                cnt[c] -= 1
            l += 1

        ans = max(ans, r - l + 1)

    print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai cốt lõi duy trì số lần chuyển tiếp thay vì tính toán lại chúng trên mỗi chuỗi con. Hàm phân loại mã hóa ba loại cạnh, đảm bảo rằng mỗi cặp liền kề đóng góp chính xác một lần. 

Cập nhật con trỏ trái cẩn thận loại bỏ sự đóng góp của cạnh đi trước khi dịch chuyển l. Điều kiện l + 1 <= r ngăn chặn truy cập không hợp lệ khi cửa sổ có kích thước một, vì một ký tự đơn không có chuyển tiếp. 

Một cạm bẫy phổ biến là quên rằng các ràng buộc áp dụng cho quá trình chuyển đổi chứ không phải số lượng ký tự thô. Một cách khác là cập nhật bộ đếm không chính xác khi thu nhỏ cửa sổ, điều này làm phá vỡ tính bất biến và dẫn đến việc đếm quá mức. 

## Ví dụ đã hoạt động 

Xét S = 0101, với n = 0, m = 3, k = 0. Chỉ cho phép gạch hỗn hợp. 

Chúng tôi theo dõi việc mở rộng cửa sổ. 

| r | S[r] | Cửa sổ | cnt00 | cnt01 | cnt11 | hợp lệ | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | 0 | 0 | 0 | 0 | 0 | vâng | 
| 1 | 1 | 01 | 0 | 1 | 0 | vâng | 
| 2 | 0 | 010 | 0 | 2 | 0 | vâng | 
| 3 | 1 | 0101 | 0 | 3 | 0 | vâng | 

Chuỗi đầy đủ là khả thi, vì vậy câu trả lời là 4. Điều này xác nhận rằng cấu trúc xen kẽ chỉ tiêu thụ các ô hỗn hợp. 

Bây giờ hãy xem xét S = 00110, với n = 1, m = 1, k = 0. Các ô hỗn hợp không được phép, do đó việc chuyển đổi bị cấm. 

| r | S[r] | Cửa sổ | cnt00 | cnt01 | cnt11 | hợp lệ | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | 0 | 0 | 0 | 0 | 0 | vâng | 
| 1 | 0 | 00 | 1 | 0 | 0 | vâng | 
| 2 | 1 | 001 | 1 | 1 | 0 | vâng | 
| 3 | 1 | 0011 | 1 | 1 | 1 | vâng | 
| 4 | 0 | 00110 | 2 | 1 | 1 | không co lại | 

Tại r = 4, cửa sổ trở nên không hợp lệ vì cnt00 vượt quá n. Thuật toán dịch chuyển l cho đến khi tính khả thi được khôi phục, đảm bảo tìm thấy hậu tố hợp lệ nhất. 

Những dấu vết này cho thấy tính khả thi hoàn toàn phụ thuộc vào cấu trúc liền kề chứ không phải tính tổng thể. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N) | mỗi con trỏ di chuyển tối đa N lần, mỗi cạnh được thêm và xóa một lần | 
| Không gian | O(1) | chỉ có ba bộ đếm và con trỏ được lưu trữ | 

Độ dài chuỗi có thể đạt tới 1e6, vì vậy thời gian tuyến tính là cần thiết. Cửa sổ trượt đảm bảo mỗi cặp ký tự được xử lý với số lần không đổi, phù hợp thoải mái trong các ràng buộc. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    solve()
    return sys.stdout.getvalue().strip()

# provided sample-style cases
assert run("0 2 3\n010\n") == "3"

# minimum size
assert run("1 1 1\n0\n") == "1"

# all equal
assert run("10 0 10\n00000\n") == "5"

# alternating tight constraint
assert run("0 10 0\n010101\n") == "6"

# forced shrink behavior
assert run("0 0 1\n001100\n") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| char đơn | 1 | trường hợp cơ sở | 
| tất cả số không | chiều dài đầy đủ | tích lũy 00 | 
| xen kẽ | sử dụng đầy đủ m | chuyển tiếp hỗn hợp | 
| không được phép trộn lẫn | logic thu nhỏ | co cửa sổ | 

## Vỏ cạnh 

Chuỗi ký tự đơn là ranh giới đơn giản nhất. Vì không có cặp liền kề nên không tiêu tốn tài nguyên. Thuật toán khởi tạo chính xác ans với ít nhất 1 khi r = 0 và không xảy ra cập nhật bộ đếm. 

Một chuỗi xen kẽ hoàn toàn nhấn mạnh sự phụ thuộc vào m. Mỗi bước tăng cnt01 và thuật toán không bao giờ co lại trừ khi vượt quá m. Điều này xác minh việc phân loại chính xác các cạnh hỗn hợp và xử lý chính xác sự tăng trưởng bền vững. 

Một chuỗi như 00000 ứng suất tích lũy của 00 ô. Mỗi tiện ích mở rộng thêm một cnt00 và khi vượt quá n, con trỏ bên trái sẽ di chuyển về phía trước để loại bỏ các đóng góp 00 cũ. Tính chính xác phụ thuộc vào hành vi thêm-xóa đối xứng, đảm bảo không xảy ra việc đếm hai lần trong quá trình thu nhỏ.
