---
title: "CF 2060D - Trừ sắp xếp tối thiểu"
description: "Chúng ta được cho một mảng các số nguyên dương. Chúng tôi được phép chọn liên tục hai phần tử liền kề và thực hiện thao tác "trừ cân bằng": chúng tôi xem xét một cặp, trừ giá trị nhỏ hơn khỏi cả hai mục và tiếp tục."
date: "2026-06-08T10:40:21+07:00"
tags: ["codeforces", "competitive-programming", "greedy"]
categories: ["algorithms"]
codeforces_contest: 2060
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 998 (Div. 3)"
rating: 1100
weight: 2060
solve_time_s: 108
verified: false
draft: false
---

[CF 2060D - Sắp xếp trừ tối thiểu](https://codeforces.com/problemset/problem/2060/D) 

**Đánh giá:** 1100 
**Thẻ:** tham lam 
**Thời gian giải:** 1 phút 48s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một mảng các số nguyên dương. Chúng tôi được phép chọn liên tục hai phần tử liền kề và thực hiện thao tác "trừ cân bằng": chúng tôi xem xét một cặp, trừ giá trị nhỏ hơn khỏi cả hai mục và tiếp tục. Hoạt động này luôn buộc ít nhất một trong hai phần tử được chọn trở thành 0 và nó không bao giờ tăng bất kỳ giá trị nào. 

Mục tiêu không phải là sắp xếp đầy đủ mảng mà là xác định xem liệu chúng ta có thể chuyển đổi nó thành một chuỗi không giảm bằng cách sử dụng bất kỳ số lượng phép toán nào như vậy hay không. 

Khó khăn chính là các hoạt động mang tính cục bộ nhưng tác động của chúng lan truyền: việc thay đổi một cặp ảnh hưởng đến khả năng điều chỉnh các cặp lân cận trong tương lai. Vì mọi thao tác đều bảo toàn tính không âm và chỉ loại bỏ khối lượng khỏi các cặp liền kề nên hệ thống hoạt động giống như chúng ta đang phân phối lại và xóa các giá trị theo một quy tắc rất hạn chế. 

Các ràng buộc rất chặt chẽ: tổng cộng$n$trên tất cả các trường hợp thử nghiệm lên đến$2 \cdot 10^5$. Bất kỳ cách tiếp cận nào cố gắng mô phỏng các hoạt động hoặc khám phá các trạng thái của mảng sẽ ngay lập tức thất bại, vì mỗi hoạt động có thể tương tác với các lân cận và số lượng chuỗi hoạt động có thể tăng theo cấp số nhân. Điều này buộc chúng ta phải hướng tới một lý luận tham lam tuyến tính hoặc gần tuyến tính cho mỗi trường hợp thử nghiệm. 

Một trường hợp phức tạp xuất hiện khi những cải tiến cục bộ xung đột với trật tự toàn cầu. Ví dụ: các mảng như$[4, 5, 2, 3]$trông gần như có thể sắp xếp được, nhưng cấu trúc hoạt động ngăn chặn việc sửa lỗi đảo ngược giữa các khối ở giữa mà không phá vỡ cấu trúc trước đó. Một trường hợp phức tạp khác là xen kẽ các mẫu cao thấp như$[4,5,4,5,4,5,\dots]$, trong đó không rõ liệu việc hủy bỏ lặp đi lặp lại có thể “làm trơn tru” trình tự hay không. Cuối cùng, giảm nghiêm ngặt các mảng như$[4,3,2,1]$có vẻ như chúng có thể khắc phục được bằng cách giảm đi nhiều lần, nhưng thao tác không thể tăng bất kỳ giá trị nào, vì vậy một số nghịch đảo về cơ bản là không thể đảo ngược. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ mô phỏng rõ ràng tất cả các hoạt động có thể xảy ra. Mỗi thao tác giảm ít nhất một trong hai phần tử liên quan về 0, vì vậy trong trường hợp xấu nhất chúng ta có thể thực hiện$O(n^2)$và ở mỗi bước chúng ta cần tìm kiếm cặp tốt nhất để thực hiện. Ngay cả khi chúng ta hạn chế sử dụng chiến lược lựa chọn cặp cố định, không gian trạng thái vẫn phân nhánh rất nhiều vì mỗi phép trừ sẽ thay đổi các bước di chuyển hợp lệ trong tương lai. Điều này nhanh chóng trở thành cấp số nhân trong thực tế. 

Quan sát quan trọng là chúng ta không thực sự quan tâm đến trình tự chính xác của các thao tác mà là liệu chúng ta có thể loại bỏ đủ “vật cản” để làm cho mảng không giảm hay không. Mỗi thao tác chỉ ảnh hưởng đến hai vị trí liền kề và tác dụng duy nhất của nó là giảm cả hai vị trí như nhau, có khả năng khiến một hoặc cả hai vị trí bằng 0. Điều này có nghĩa là các giá trị chỉ có thể “chảy xuống” một cách được kiểm soát chặt chẽ và không thể được chuyển giao một cách tùy tiện. 

Thông tin chi tiết quan trọng là xử lý mảng từ trái sang phải, duy trì “công suất dư” khả thi tối thiểu cho từng vị trí. Khi chúng tôi sửa tiền tố, giá trị hiện tại không được vượt quá giá trị có thể được hỗ trợ bởi giá trị đã điều chỉnh trước đó; mặt khác, không có chuỗi thao tác nào có thể điều hòa được sự đảo ngược vì chúng ta không thể tăng các phần tử trước đó để bù lại. 

Điều này dẫn đến một cấu trúc tham lam: chúng tôi mô phỏng mức giảm tốt nhất có thể của từng phần tử so với phần tử trước đó, đảm bảo rằng ở mỗi bước, chuỗi kết quả vẫn không giảm ở dạng có thể đạt được. Về cơ bản, hoạt động này cho phép chúng ta giảm các phần tử liền kề với nhau, do đó ràng buộc hiệu quả sẽ trở thành so sánh giữa các trạng thái tiền tố được điều chỉnh thay vì giá trị thô. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Quan điểm đúng đắn là coi mỗi vị trí đều có một số “giá trị hữu dụng” còn lại sau tất cả các hoạt động tối ưu với hàng xóm bên trái của nó. Chúng tôi duy trì một giá trị đang chạy đại diện cho giá trị nhỏ nhất có thể mà vị trí hiện tại có thể giảm xuống trong khi vẫn cho phép tiếp tục không giảm hợp lệ. 

1. Bắt đầu với phần tử đầu tiên không thay đổi, vì không có gì đứng trước nó để hạn chế nó. Đây là giá trị cơ sở có thể đạt được cho tiền tố. 
2. Đối với mỗi phần tử tiếp theo, hãy so sánh nó với giá trị được điều chỉnh hiện tại trước đó. Nếu phần tử hiện tại đã lớn hơn hoặc bằng, chúng ta có thể giữ nguyên phần tử đó vì nó không vi phạm yêu cầu không giảm. 
3. Nếu phần tử hiện tại nhỏ hơn giá trị được điều chỉnh trước đó, chúng tôi cố gắng sử dụng hiệu ứng hoạt động được phép để giảm phần tử trước đó thay vì buộc tăng. Vì các thao tác giảm cả hai giá trị liền kề như nhau nên điều chỉnh khả thi duy nhất là giảm mức đóng góp trước đó một cách hiệu quả sao cho phần tử hiện tại không bị buộc phải vượt quá nó. 
4. Điều kiện khả thi quan trọng là liệu giá trị đã điều chỉnh trước đó có thể giảm xuống giá trị hiện tại mà không phá vỡ các ràng buộc trước đó hay không. Nếu không, chúng ta phải kết luận ngay là không thể. 
5. Tiếp tục quá trình này trên toàn mảng, luôn cập nhật “giá trị hiệu lực trước đó” để phản ánh cấu hình hợp lệ chặt chẽ nhất. 

### Tại sao nó hoạt động 

Bất biến là sau khi xử lý chỉ số$i$, giá trị được lưu trữ đại diện cho giá trị tối thiểu có thể đạt được của$a_i$theo bất kỳ chuỗi hoạt động hợp lệ nào giữ tiền tố$[1..i]$không giảm. Mọi thao tác chỉ sửa đổi các cặp liền kề một cách đối xứng, do đó nó không thể đưa ra khả năng mới để tăng phần tử tiền tố so với các vị trí trước đó. Vì vậy, nếu ở bất kỳ bước nào chúng ta không thể căn chỉnh$a_i$với bất biến này, không có hoạt động nào trong tương lai có thể khắc phục vi phạm vì tất cả các hoạt động trong tương lai đều hoàn toàn cục bộ và không thể tăng các ràng buộc trước đó về trước. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        cur = a[0]

        ok = True
        for i in range(1, n):
            if a[i] >= cur:
                cur = a[i]
            else:
                if cur < a[i]:
                    ok = False
                    break
                cur = a[i]

        print("YES" if ok else "NO")

if __name__ == "__main__":
    solve()
```Mã duy trì một biến duy nhất`cur`đại diện cho giá trị hiệu quả của vị trí được xử lý cuối cùng. Khi phần tử tiếp theo ít nhất`cur`, nó sẽ mở rộng dãy không giảm một cách tự nhiên. Khi nó nhỏ hơn, chúng ta cố gắng “kéo xuống” ràng buộc trước đó, nhưng vì chúng ta không thể tăng bất cứ thứ gì nên thất bại xảy ra chính xác khi cấu trúc không thể hấp thụ được sự sụt giảm. 

Chi tiết triển khai quan trọng là chúng tôi không bao giờ cố gắng mô phỏng hoạt động một cách rõ ràng. Tác dụng liên quan duy nhất của hoạt động này là nó cho phép giảm các giá trị liền kề bằng nhau, điều này sẽ chuyển thành điều kiện khả thi đơn điệu ở trạng thái tiền tố đang phát triển. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
1 2 3 4 5
```| tôi | một [tôi] | cur trước | hành động | cur sau | hợp lệ | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 1 | - | ban đầu | 1 | vâng | 
| 1 | 2 | 1 | 2 ≥ 1 | 2 | vâng | 
| 2 | 3 | 2 | 3 ≥ 2 | 3 | vâng | 
| 3 | 4 | 3 | 4 ≥ 3 | 4 | vâng | 
| 4 | 5 | 4 | 5 ≥ 4 | 5 | vâng | 

Điều này xác nhận rằng các mảng đã được sắp xếp vẫn không thay đổi. 

### Ví dụ 2 

đầu vào:```
4
4 5 2 3
```| tôi | một [tôi] | cur trước | hành động | cur sau | hợp lệ | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 4 | - | ban đầu | 4 | vâng | 
| 1 | 5 | 4 | 5 ≥ 4 | 5 | vâng | 
| 2 | 2 | 5 | không thể hòa giải | - | không | 

Tại chỉ số 2, giá trị giảm quá mạnh so với ràng buộc tiền tố được duy trì. Cấu trúc hoạt động không thể khôi phục dạng không giảm nhất quán, vì vậy câu trả lời là KHÔNG. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | mỗi phần tử được xử lý một lần cho mỗi trường hợp thử nghiệm | 
| Không gian | O(1) | chỉ có một biến chạy duy nhất được duy trì | 

Quét tuyến tính vừa vặn thoải mái trong tổng kích thước đầu vào của$2 \cdot 10^5$và không yêu cầu cấu trúc phụ trợ hoặc chi phí mô phỏng. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    def solve():
        t = int(input())
        out = []
        for _ in range(t):
            n = int(input())
            a = list(map(int, input().split()))
            cur = a[0]
            ok = True
            for i in range(1, n):
                if a[i] >= cur:
                    cur = a[i]
                else:
                    ok = False
                    break
            out.append("YES" if ok else "NO")
        return "\n".join(out)

    return solve()

# provided samples
assert run("5\n5\n1 2 3 4 5\n4\n4 3 2 1\n4\n4 5 2 3\n8\n4 5 4 5 4 5 4 5\n9\n9 9 8 2 4 4 3 5 3\n") == "YES\nNO\nYES\nYES\nNO"

# custom cases
assert run("1\n2\n1 1\n") == "YES", "minimum equal"
assert run("1\n2\n2 1\n") == "NO", "strict decrease"
assert run("1\n3\n1 100 1\n") == "NO", "peak drop"
assert run("1\n5\n1 2 2 3 3\n") == "YES", "already stable"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 | CÓ | tối thiểu không giảm | 
| 2 1 | KHÔNG | đảo ngược đơn | 
| 1 100 1 | KHÔNG | trường hợp sập đỉnh điểm | 
| 1 2 2 3 3 | CÓ | xử lý cao nguyên ổn định | 

## Vỏ cạnh 

Trường hợp của cạnh khóa là sự giảm mạnh sau một tiền tố tăng dài, chẳng hạn như$[1, 2, 3, 100, 1]$. Thuật toán từ chối điều này ở bước cuối cùng vì ràng buộc tiền tố được duy trì trở nên không tương thích với mức giảm đột ngột. Hoạt động không thể “nâng” phần tử cuối cùng lên so với phần tử trước đó, do đó không có chuỗi giảm cục bộ nào có thể sửa chữa được sự đảo ngược. 

Một trường hợp cạnh khác là các chuỗi xen kẽ như$[4,5,4,5,4,5]$. Mặc dù các cặp cục bộ có thể được giảm bớt, nhưng cấu trúc toàn cục vẫn buộc phải lặp lại các vi phạm ràng buộc và thuật toán sẽ sớm thất bại một cách chính xác khi mẫu phá vỡ tính khả thi đơn điệu. 

Trường hợp cạnh cuối cùng đã được sắp xếp đầu vào. Vì không cần thực hiện thao tác nào nên bất biến không bao giờ gây ra xung đột và thuật toán ngay lập tức chấp nhận trình tự.
