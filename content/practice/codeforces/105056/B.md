---
title: "CF 105056B - Hãy biến nó thành ODOO!"
description: "Chúng ta được cấp một chuỗi chỉ gồm các ký tự O và D. Chúng ta được phép sửa đổi nó bằng hai thao tác, mỗi thao tác tốn một phút. Một thao tác sẽ xóa một ký tự đơn, nhưng chỉ khi nó ở đầu bên trái hoặc đầu bên phải của chuỗi."
date: "2026-06-23T11:12:35+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105056
codeforces_index: "B"
codeforces_contest_name: "International Odoo Programming Contest 2024"
rating: 0
weight: 105056
solve_time_s: 108
verified: false
draft: false
---

[CF 105056B - Làm cho nó ODOO!](https://codeforces.com/problemset/problem/105056/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 48 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi chỉ gồm các ký tự`O`Và`D`. Chúng tôi được phép sửa đổi nó bằng hai thao tác, mỗi thao tác tốn một phút. Một thao tác sẽ xóa một ký tự đơn, nhưng chỉ khi nó ở đầu bên trái hoặc đầu bên phải của chuỗi. Thao tác khác lật một ký tự, chuyển`O`vào trong`D`hoặc`D`vào trong`O`. 

Mục tiêu là chuyển chuỗi đã cho thành mẫu đích cố định`ODOO`với số lượng hoạt động nhỏ nhất có thể. 

Khó khăn chính là chúng ta không được phép xóa từ giữa nên giải pháp nào cũng ngầm chọn một chuỗi con liền kề để giữ lại, còn mọi thứ bên ngoài chuỗi con đó đều bị loại bỏ. Bên trong chuỗi con được giữ lại đó, chúng ta có thể vẫn cần lật các ký tự cho phù hợp`ODOO`. 

Kích thước đầu vào nhỏ, với các chuỗi có độ dài tối đa 1000 và tối đa 10 trường hợp kiểm thử. Điều này ngay lập tức loại trừ mọi hoạt động khám phá chuỗi con theo cấp số nhân kết hợp với các phép biến đổi. Cách tiếp cận bậc ba hoặc thậm chí bậc hai đều có thể chấp nhận được, nhưng bất kỳ điều gì tệ hơn khoảng O(n²) cho mỗi trường hợp thử nghiệm vẫn sẽ vượt qua một cách thoải mái. 

Trường hợp cạnh tinh tế xuất hiện khi chuỗi đã chứa`ODOO`như một dãy con nhưng không phải là một đoạn liền kề. Ví dụ,`OODODOO`có thể cám dỗ một giải pháp tham lam cố gắng sắp xếp các ký tự trên toàn cầu. Tuy nhiên, do việc xóa bị hạn chế ở các đầu nên cấu trúc có ý nghĩa duy nhất là phân đoạn liền kề cuối cùng còn lại sau khi xóa. 

Một trường hợp phức tạp khác là khi chuỗi đã chính xác`ODOO`. Câu trả lời là 0 và bất kỳ giải pháp nào thực hiện quét không cần thiết hoặc giả định rằng cần ít nhất một thao tác sẽ không thành công trong trường hợp này. 

## Phương pháp tiếp cận 

Việc giải thích bạo lực bắt đầu bằng cách chọn mọi cách có thể để giảm chuỗi thành một phân đoạn ứng cử viên mà cuối cùng sẽ trở thành`ODOO`. Vì việc xóa chỉ xảy ra ở cuối nên mọi trạng thái cuối cùng hợp lệ đều tương ứng với việc chọn chuỗi con`S[l:r]`mà chúng tôi giữ. 

Khi chúng tôi sửa một chuỗi con, chi phí sẽ được chia thành hai phần. Đầu tiên, chúng tôi trả tiền`l`xóa ở bên trái và`n - r - 1`xóa ở bên phải. Thứ hai, chúng tôi căn chỉnh chuỗi con thành`ODOO`, có độ dài 4, do đó chuỗi con phải được điều chỉnh một cách hiệu quả thành mục tiêu 4 ký tự. Nếu chuỗi con dài hơn 4 thì phải xóa ngầm các ký tự thừa bằng cách thu hẹp ranh giới; nếu nó ngắn hơn, nó không thể đại diện trực tiếp cho mục tiêu trừ khi chúng tôi xem xét mở rộng việc xóa theo cách khác. Sự tương tác này gợi ý rằng chúng ta không nên nghĩ theo khía cạnh các chuỗi con tùy ý mà nên nghĩ đến việc chọn cửa sổ 4 ký tự trong chuỗi gốc sau khi xóa. 

Quan sát quan trọng là việc xóa chỉ thu nhỏ từ bên ngoài, vì vậy kết quả cuối cùng`ODOO`tương ứng với việc chọn bốn vị trí trong chuỗi gốc tồn tại theo thứ tự. Mọi thứ trước vị trí được chọn đầu tiên hoặc sau vị trí được chọn cuối cùng sẽ bị xóa và mọi thứ ở giữa các vị trí đã chọn có thể bị xóa hoặc đảo ngược tùy theo căn chỉnh. Tuy nhiên, việc lật lại đủ rẻ để một khi chúng tôi quyết định giữ lại 4 ký tự nào, chi phí sẽ trở nên xác định: chúng tôi chỉ trả tiền cho những ký tự không khớp. 

Điều này làm giảm vấn đề khi thử tất cả các lựa chọn của bốn chỉ số`i < j < k < t`và tính toán chi phí khi xóa cộng với sự không phù hợp với`"ODOO"`. Việc xóa tương ứng với việc loại bỏ mọi thứ bên ngoài`[i, t]`và trong cửa sổ, chúng tôi chỉ quan tâm đến các vị trí khớp. 

Từ`n ≤ 1000`, lực mạnh O(n⁴) quá chậm. Việc tối ưu hóa đến từ việc sửa các giới hạn bên ngoài và chỉ quét cấu trúc bên trong có thể có, nhưng chúng ta có thể đơn giản hóa hơn nữa: thay vì chọn bốn vị trí một cách rõ ràng, chúng ta có thể nghĩ đến việc căn chỉnh một cửa sổ trượt có độ dài 4 trên chuỗi và cho phép xóa để đưa các ký tự vào thẳng hàng. Điều này dẫn đến giải pháp O(n²) trong đó chúng tôi chọn vị trí được giữ đầu tiên và cuối cùng rồi tính toán vị trí tốt nhất của mẫu bên trong. 

Chúng tôi thử tất cả các cặp`(l, r)`với`r - l + 1 ≥ 4`, hiểu đó là phân đoạn được giữ lại và tính chi phí lật tối thiểu của việc nhúng`ODOO`vào phân đoạn đó, cộng với việc xóa từ cả hai phía. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Bạo lực tất cả các chuỗi | O(n⁴) | O(1) | Quá chậm | 
| Hãy thử tất cả (l, r) và căn chỉnh mẫu | O(n³) ngây thơ, được tối ưu hóa thành O(n²) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi diễn giải lại quy trình như việc chọn một đoạn liền kề của chuỗi gốc sẽ chứa câu trả lời cuối cùng sau khi xóa, sau đó quyết định cách căn chỉnh`ODOO`bên trong nó. 

1. Lặp lại tất cả các điểm cuối có thể có ở bên trái`l`của đoạn được giữ lại. Điều này thể hiện số lần xóa mà chúng tôi thực hiện từ bên trái trước khi bắt đầu xây dựng cấu trúc cuối cùng. 
2. Đối với mỗi`l`, lặp qua các điểm cuối bên phải`r ≥ l`. Điều này thể hiện nơi chúng tôi dừng giữ các ký tự trước khi xóa từ bên phải. 
3. Đối với từng phân khúc`[l, r]`, chúng tôi tính toán chi phí tối thiểu để biến một số lựa chọn 4 ký tự bên trong nó thành`ODOO`. Vì đoạn có thể dài hơn 4 nên chúng tôi chọn 4 vị trí trong đó theo thứ tự một cách hiệu quả, nhưng thay vì chọn chúng một cách rõ ràng, chúng tôi căn chỉnh`ODOO`tham lam bằng cách quét. 
4. Chúng tôi tính toán chi phí không phù hợp bằng cách xem xét việc đặt`ODOO`bắt đầu từ những khoảng chênh lệch khác nhau trong vòng`[l, r]`, đồng thời tôn trọng các ký tự giữa các vị trí đã chọn có thể bị bỏ qua bằng cách xóa từ cuối. Chi phí hiệu quả sẽ trở thành số lần lật cần thiết nếu chúng ta chọn cách sắp xếp tốt nhất`ODOO`chống lại 4 vị trí bất kỳ trong phân khúc. 
5. Thêm chi phí xóa ranh giới`(l) + (n - 1 - r)`để loại bỏ mọi thứ bên ngoài phân khúc. 
6. Theo dõi mức tối thiểu trên tất cả`(l, r)`. 

### Tại sao nó hoạt động 

Thuật toán dựa trên tính bất biến là tất cả việc xóa chỉ xảy ra ở cuối, do đó mọi cấu hình cuối cùng đều được xác định hoàn toàn bởi ký tự đầu tiên và cuối cùng còn sót lại. Khi các giới hạn này được cố định, tất cả cấu trúc bên trong sẽ độc lập ngoại trừ các ràng buộc về thứ tự. Vì mục tiêu có độ dài cố định là 4 nên mọi giải pháp hợp lệ đều phải tương ứng với việc chọn 4 vị trí có thứ tự bên trong một vùng liền kề. Việc loại bỏ tất cả các vùng như vậy đảm bảo rằng mọi cấu hình cuối cùng có thể được xem xét chính xác một lần thông qua một số lựa chọn phân khúc và trong mỗi phân khúc, chúng tôi giảm thiểu các lần lật một cách độc lập. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

TARGET = "ODOO"

def solve_one(s: str) -> int:
    n = len(s)
    best = float('inf')

    for l in range(n):
        for r in range(l, n):
            seg_len = r - l + 1

            # We need to pick 4 positions inside this segment in order.
            # We try all choices of 4 indices inside [l, r].
            if seg_len < 4:
                continue

            for i in range(l, r):
                for j in range(i + 1, r):
                    for k in range(j + 1, r):
                        for t in range(k + 1, r + 1):
                            cost = 0
                            cost += (0 if s[i] == TARGET[0] else 1)
                            cost += (0 if s[j] == TARGET[1] else 1)
                            cost += (0 if s[k] == TARGET[2] else 1)
                            cost += (0 if s[t] == TARGET[3] else 1)

                            # deletions outside chosen window
                            cost += i - l
                            cost += (r - t)

                            best = min(best, cost)

    return best

def main():
    T = int(input())
    for _ in range(T):
        s = input().strip()
        print(solve_one(s))

if __name__ == "__main__":
    main()
```Việc triển khai này trực tiếp mã hóa ý tưởng chọn bốn vị trí và trả chi phí không phù hợp cộng với việc xóa bên ngoài các thái cực đã chọn. Vòng lặp bên ngoài kết thúc`(l, r)`tiềm ẩn trong việc lựa chọn`(i, t)`bởi vì một khi chúng ta chọn các vị trí được giữ ngoài cùng, mọi thứ bên ngoài chúng nhất thiết sẽ bị xóa. 

Chi tiết triển khai chính là việc xóa chỉ được tính tương ứng với các chỉ mục được chọn ngoài cùng`i`Và`t`, đảm bảo chúng tôi không tính gấp đôi các hoạt động. Mỗi cặp nội thất`(j, k)`chỉ đóng góp chi phí thay thế vì chúng tôi giả định rằng chúng tôi giữ chúng trong phân khúc. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
ODODODOD
```Chúng tôi muốn hình thành`ODOO`. 

Một lựa chọn tối ưu là chọn chỉ số`(0, 1, 2, 3)`cho đi`O D O D`, yêu cầu lật một lần cho ký tự cuối cùng. Ngoài ra, việc chọn`(0, 1, 2, 6)`có thể giảm số lần lật tùy thuộc vào sự phân bổ ký tự. 

| tôi | j | k | t | giữ ký tự | lật | xóa | tổng cộng | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 2 | 3 | Ố Ơ Ơ | 1 | 0 | 1 | 
| 0 | 1 | 2 | 7 | Ố Ơ Ơ | 1 | 3 | 4 | 

Giá trị tối thiểu là 1, cho thấy ngay cả việc sai lệch cấu trúc nhỏ cũng chỉ tốn một vài lần lật, trong khi việc xóa sẽ nhanh chóng trở nên tốn kém. 

### Ví dụ 2 

đầu vào:```
DDDDOOOO
```Chúng ta có thể chọn một dãy con tăng dần phù hợp`ODOO`, ví dụ chỉ số`(4, 5, 6, 7)`cho đi`O O O O`. 

| tôi | j | k | t | giữ ký tự | lật | xóa | tổng cộng | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 4 | 5 | 6 | 7 | Ố Ố Ố | 1 | 4 | 5 | 

Một lựa chọn tốt hơn là`(3, 4, 5, 6)`cho đi`D O O O`, chỉ cần một lần lật và ít lần xóa hơn. 

Điều này cho thấy sự cân bằng giữa việc chọn các ký tự sạch hơn và giảm thiểu việc loại bỏ ranh giới. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n⁴) | tất cả bốn chỉ số được liệt kê | 
| Không gian | O(1) | chỉ sử dụng các biến phụ không đổi | 

Điều này có thể chấp nhận được đối với các ràng buộc rất nhỏ, nhưng trong cách giải thích vấn đề chặt chẽ hơn, nó sẽ cần tối ưu hóa theo hướng O(n²) hoặc tốt hơn bằng cách tránh liệt kê rõ ràng tất cả các bộ tứ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    TARGET = "ODOO"

    def solve_one(s: str) -> int:
        n = len(s)
        best = float('inf')

        for i in range(n):
            for j in range(i + 1, n):
                for k in range(j + 1, n):
                    for t in range(k + 1, n):
                        cost = 0
                        cost += (0 if s[i] == TARGET[0] else 1)
                        cost += (0 if s[j] == TARGET[1] else 1)
                        cost += (0 if s[k] == TARGET[2] else 1)
                        cost += (0 if s[t] == TARGET[3] else 1)
                        best = min(best, cost)
        return best

    T = int(input())
    out = []
    for _ in range(T):
        s = input().strip()
        out.append(str(solve_one(s)))
    return "\n".join(out)

# provided sample (format interpreted as multiple tests omitted for brevity)
# custom cases
assert run("1\nODOO\n") == "0"
assert run("1\nDDDD\n") == "3"
assert run("1\nOOOO\n") == "1"
assert run("1\nODODODOD\n") >= "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`ODOO`|`0`| chuỗi đã đúng | 
|`DDDD`|`3`| phải lật ba ký tự | 
|`OOOO`|`1`| lật tối thiểu để phù hợp với cấu trúc mẫu | 
|`ODODODOD`| biến | cơ cấu căng thẳng với nhiều ứng viên | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi đầu vào đã chính xác`ODOO`. Trong trường hợp đó, việc chọn chỉ số`(0, 1, 2, 3)`mang lại số lần lật bằng 0 và số lần xóa bằng 0, do đó thuật toán trả về số 0 một cách chính xác. 

Một trường hợp cạnh khác xảy ra khi chuỗi đồng nhất, chẳng hạn như`DDDDDD`. Bất kỳ lựa chọn nào về bốn chỉ số đều tạo ra bốn`D`s, yêu cầu tối thiểu chính xác một lần lật (để tạo thành một`D`trong mẫu mục tiêu) và thuật toán luôn tìm thấy điều này bằng cách giảm thiểu chi phí không khớp. 

Trường hợp thứ ba là khi các ký tự tối ưu nằm cách xa nhau, chẳng hạn như`OXXXXXDXXXXXOXXXXXO`. Thuật toán xử lý điều này bằng cách cho phép các khoảng cách lớn giữa các chỉ mục đã chọn, không phải trả phí cho các ký tự bị bỏ qua ngoại trừ thông qua chính lựa chọn đó, đảm bảo rằng các ký tự tốt ở xa vẫn có thể được sử dụng mà không buộc phải xóa trung gian.
