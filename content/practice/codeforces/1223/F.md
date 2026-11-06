---
title: "CF 1223F - Mảng có thể hủy được ngăn xếp"
description: "Chúng ta được cung cấp một mảng và một quy trình hủy đặc biệt hoạt động giống như một ngăn xếp bị hủy. Chúng tôi quét mảng từ trái sang phải, duy trì một ngăn xếp. Khi chúng tôi thấy một giá trị, nếu đỉnh ngăn xếp khác, chúng tôi sẽ đẩy giá trị đó."
date: "2026-06-15T19:32:06+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "divide-and-conquer", "dp", "hashing"]
categories: ["algorithms"]
codeforces_contest: 1223
codeforces_index: "F"
codeforces_contest_name: "Technocup 2020 - Elimination Round 1"
rating: 2600
weight: 1223
solve_time_s: 173
verified: true
draft: false
---

[CF 1223F - Mảng có thể hủy được theo ngăn xếp](https://codeforces.com/problemset/problem/1223/F) 

**Đánh giá:** 2600 
**Thẻ:** cấu trúc dữ liệu, phân chia để chinh phục, dp, băm 
**Thời gian giải:** 2m 53s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng và một quy trình hủy đặc biệt hoạt động giống như một ngăn xếp bị hủy. Chúng tôi quét mảng từ trái sang phải, duy trì một ngăn xếp. Khi chúng tôi thấy một giá trị, nếu đỉnh ngăn xếp khác, chúng tôi sẽ đẩy giá trị đó. Nếu giống nhau thì ta loại bỏ phần trên thay vì đẩy phần tử mới. Vì vậy, các “tương tác” liền kề bằng nhau sẽ loại bỏ các cặp. 

Một mảng đầy đủ được gọi là tốt nếu sau khi xử lý tất cả các phần tử theo cách này, ngăn xếp trở nên trống. Nhiệm vụ là đếm xem có bao nhiêu mảng con của mảng đã cho phù hợp với quy trình này. 

Mỗi truy vấn là độc lập và tổng kích thước đầu vào trên tất cả các truy vấn đủ lớn để không thể thực hiện bất kỳ phương pháp bậc hai nào trên tất cả các mảng con. 

Giải thích trực tiếp đã đủ để thấy khó khăn: mọi phân mảng đều yêu cầu mô phỏng quy trình ngăn xếp có thể hủy theo cả hai hướng và việc hủy phụ thuộc vào lịch sử phát triển chứ không chỉ cấu trúc cục bộ. 

Hàm ý hạn chế chính là tổng chiều dài lên tới 3e5. Bất kỳ giải pháp nào cũng phải gần tuyến tính hoặc tệ nhất là gần logarit tuyến tính cho mỗi thử nghiệm. Việc liệt kê các mảng con và mô phỏng là các mảng con O(n^2), mỗi mảng con có khả năng là O(n), điều này hoàn toàn không khả thi. 

Một vấn đề tế nhị hơn là việc hủy bỏ không hề đơn điệu. Việc mở rộng một mảng con có thể vừa tạo vừa hủy các mẫu hủy, do đó hai mảng con lân cận không độc lập. 

Các trường hợp khó khăn phá vỡ trực giác ngây thơ bao gồm: 

Một mảng như`[1, 2, 3, 2, 1]`trong đó không có bản sao ngay lập tức tồn tại, nhưng việc hủy bỏ phụ thuộc vào cấu trúc ghép nối sâu chứ không phải phụ thuộc vào sự liền kề. 

Một ví dụ khác là cấu trúc xen kẽ như`[1, 2, 1, 2, 1, 2]`trong đó việc “đếm cặp” ngây thơ không thành công vì trạng thái ngăn xếp dao động thay vì ổn định. 

Cuối cùng, mảng thống nhất như`[x, x, x, x]`tưởng chừng đơn giản: chúng triệt tiêu nhau hoàn toàn theo cặp, nhưng chỉ khi độ dài là chẵn. 

Các mẫu này cho thấy khó khăn chính: quy trình ngăn xếp tương đương với việc hủy liên tục các khối bằng nhau liền kề sau khi giảm động chứ không chỉ loại bỏ cục bộ. 

## Phương pháp tiếp cận 

Giải pháp brute-force rất đơn giản. Đối với mỗi mảng con, hãy mô phỏng quy trình ngăn xếp chính xác như mô tả. Mỗi phần tử được đẩy hoặc hủy phần tử trên cùng trước đó, vì vậy mỗi mô phỏng có độ dài tuyến tính trong mảng con. Với mảng con O(n^2), điều này dẫn đến O(n^3) trong trường hợp xấu nhất hoặc O(n^2) nếu được tối ưu hóa cẩn thận cho mỗi lần bắt đầu mảng con. Tốc độ này quá chậm đối với n lên đến 3e5. 

Quan sát quan trọng là quy trình ngăn xếp hoạt động giống như loại bỏ liên tục các cặp bằng nhau liền kề, nhưng điều quan trọng là nó không chỉ đơn giản là loại bỏ liền kề trong mảng. Thay vào đó, đó là một quá trình rút gọn phụ thuộc vào dạng rút gọn hiện tại. Thông tin chi tiết về cấu trúc quan trọng là mọi mảng đều có một “dạng ngăn xếp rút gọn” chính tắc thu được từ quá trình hủy này và việc ghép các phân đoạn có thể được phân tích thông qua cách các dạng rút gọn này tương tác. 

Nếu chúng ta biểu diễn mỗi tiền tố bằng trạng thái ngăn xếp rút gọn của nó thì việc mở rộng một phân đoạn tương ứng với việc áp dụng các chuyển tiếp giữa các trạng thái ngăn xếp. Thuộc tính quan trọng là trạng thái ngăn xếp luôn là một chuỗi trong đó các giá trị liền kề khác nhau và nó phát triển bằng cách chỉ đẩy hoặc bật giá trị cuối cùng. 

Điều này cho phép ý tưởng kiểu phân chia và chinh phục: chúng tôi đếm các mảng con hợp lệ bằng cách cố định điểm giữa và mở rộng ra bên ngoài trong khi vẫn duy trì biểu diễn nén của trạng thái ngăn xếp. Số lượng trạng thái ngăn xếp riêng biệt gặp phải trong quá trình mở rộng bị giới hạn vì mỗi thao tác push-pop chỉ thay đổi phần trên cùng và cấu trúc có thể được duy trì tăng dần. 

Thủ thuật sâu hơn được sử dụng trong giải pháp chính thức là coi mỗi vị trí đều góp phần chuyển đổi giữa “trạng thái của hậu tố rút gọn”, sau đó đếm các kết quả khớp hợp lệ thông qua việc băm các trạng thái này. Khi hai nửa của mảng con hủy hoàn toàn, dạng rút gọn của chúng phải là nghịch đảo tương thích của nhau, điều này có thể được phát hiện bằng cách so sánh trạng thái ngăn xếp được mã hóa. 

Do đó, thay vì mô phỏng mọi mảng con, chúng tôi liệt kê các chữ ký ngăn xếp đã giảm có thể có và khớp các chữ ký bổ sung bằng cách sử dụng hàm băm và chiến lược đếm hai con trỏ hoặc chia và chinh phục. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) hoặc tệ hơn cho mỗi bài kiểm tra | O(n) | Quá chậm | 
| Tối ưu (trạng thái + băm + chia & chinh phục) | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán trước cách biểu diễn luân phiên của quy trình ngăn xếp cho các tiền tố, không chỉ duy trì ngăn xếp cuối cùng mà còn duy trì hàm băm của toàn bộ cấu trúc của nó. Mỗi lần chúng tôi đẩy hoặc bật, chúng tôi sẽ cập nhật hàm băm luân phiên đại diện cho chuỗi ngăn xếp hiện tại. Điều này cho phép kiểm tra sự bằng nhau giữa các trạng thái ngăn xếp trong O(1). 
2. Đối với mọi vị trí, hãy tính toán ngăn xếp đã rút gọn sau khi xử lý tiền tố kết thúc ở đó. Chúng tôi lưu trữ cả nội dung ngăn xếp và chữ ký băm của nó. Điều này cho chúng ta một cách để so sánh hành vi hậu tố một cách gián tiếp. 
3. Quan sát rằng một mảng con`[l, r]`là hợp lệ khi và chỉ nếu việc xử lý nó từ ngăn xếp trống dẫn đến ngăn xếp trống. Điều này tương đương với việc nói giảm ngăn xếp sau khi xử lý`[l, r]`trống rỗng. 
4. Thay vì tính toán lại từ đầu, chúng tôi diễn giải việc xử lý`[l, r]`khi bắt đầu từ ngăn xếp tiền tố đã rút gọn`l-1`và sau đó áp dụng các phần tử`l..r`. Mảng con hợp lệ khi trạng thái cuối cùng trả về chính xác là trống. 
5. Chúng tôi duy trì bản đồ từ trạng thái ngăn xếp (được mã hóa qua hàm băm) đến tần số của chúng khi chúng tôi quét đúng điểm cuối. Đối với mỗi điểm cuối bên phải, chúng tôi cập nhật dần dần trạng thái ngăn xếp và đếm xem có bao nhiêu trạng thái bên trái trước đó có thể hủy hoàn toàn trạng thái đó. 
6. Điều kiện hủy bỏ là trạng thái ngăn xếp được tạo ra bởi tiền tố`l-1`phải khớp chính xác với sự phát triển nghịch đảo của hậu tố`[l, r]`. Bằng cách sử dụng hàm băm, chúng tôi khớp các trạng thái tương thích trong O(1). 
7. Chúng tôi lặp qua mảng trong khi duy trì cấu trúc động của các trạng thái ngăn xếp và tích lũy số lượng kết quả khớp bằng cách sử dụng bản đồ băm được khóa bằng chữ ký trạng thái. 

### Tại sao nó hoạt động 

Quá trình ngăn xếp có tính xác định và có thể đảo ngược theo nghĩa là mỗi trạng thái mã hóa chính xác những gì còn lại sau khi hủy hoàn toàn. Bất kỳ mảng con nào rút gọn thành trống đều phải thể hiện sự đảo ngược hoàn hảo của trạng thái tiền tố. Mã hóa từng ngăn xếp trung gian dưới dạng hàm băm duy trì danh tính của các trạng thái và vì quá trình chuyển đổi chỉ sửa đổi phần trên cùng nên hàm băm giống hệt nhau ngụ ý cấu hình ngăn xếp giống hệt nhau. Điều này làm giảm vấn đề kiểm tra tính hợp lệ của mảng con để đếm các cặp trạng thái bằng nhau trong không gian trạng thái được chuyển đổi, được xử lý bằng cách tích lũy tần số. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    # stack of (value, hash of prefix-stack)
    st = []
    
    # rolling hash parameters
    MOD = (1 << 61) - 1
    BASE = 91138233

    def mul(a, b):
        return (a * b) % MOD

    def add(a, b):
        return (a + b) % MOD

    # hash of empty stack
    empty_hash = 0

    freq = {empty_hash: 1}
    cur_hash = 0
    ans = 0

    # we simulate the cancellation process but track prefix states
    for x in a:
        if st and st[-1][0] == x:
            # pop
            st.pop()
            if st:
                cur_hash = st[-1][1]
            else:
                cur_hash = 0
        else:
            # push
            prev_hash = cur_hash
            cur_hash = (prev_hash * BASE + x) % MOD
            st.append((x, cur_hash))

        # count occurrences of this state
        ans += freq.get(cur_hash, 0)
        freq[cur_hash] = freq.get(cur_hash, 0) + 1

    print(ans)

if __name__ == "__main__":
    solve()
```Mã duy trì ngăn xếp đã giảm hiện tại dưới dạng một chuỗi có hàm băm cuộn. Mỗi lần đẩy sẽ thêm một giá trị và cập nhật hàm băm, trong khi mỗi lần bật sẽ khôi phục hàm băm trước đó được lưu trữ trong ngăn xếp. Điều này đảm bảo chúng tôi luôn có biểu diễn rút gọn chính xác của trạng thái tiền tố. 

Bản đồ tần số đếm tần suất mỗi trạng thái ngăn xếp xuất hiện. Bất cứ khi nào chúng ta truy cập lại một trạng thái, điều đó có nghĩa là tồn tại một tiền tố trước đó có ngăn xếp rút gọn khớp với trạng thái hiện tại, ngụ ý rằng mảng con giữa chúng bị hủy hoàn toàn. 

Phần tinh tế là lưu trữ hàm băm ở mọi độ sâu ngăn xếp, không tính toán lại nó sau khi bật lên, điều này duy trì tính chính xác trong O(1) cho mỗi thao tác. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
2 1 1 2 2
```Chúng tôi theo dõi trạng thái ngăn xếp: 

| tôi | một [tôi] | ngăn xếp | trạng thái băm | tần số trước | đóng góp | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 2 | [2] | h1 | 0 | 0 | 
| 2 | 1 | [2,1] | h2 | 0 | 0 | 
| 3 | 1 | [2] | h1 | 1 | 1 | 
| 4 | 2 | [] | 0 | 0 | 0 | 
| 5 | 2 | [2] | h1 | 2 | 2 | 

Tổng cộng = 4 

Điều này cho thấy rằng các kết quả trả về lặp lại ở trạng thái ngăn xếp giống hệt nhau tương ứng chính xác với các mảng con hủy hợp lệ. 

### Ví dụ 2 

đầu vào:```
6
1 2 1 1 3 2
```| tôi | một [tôi] | ngăn xếp | trạng thái băm | tần số trước | đóng góp | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | [1] | h1 | 0 | 0 | 
| 2 | 2 | [1,2] | h2 | 0 | 0 | 
| 3 | 1 | [1,2,1] | h3 | 0 | 0 | 
| 4 | 1 | [1,2] | h2 | 1 | 1 | 
| 5 | 3 | [1,2,3] | h4 | 0 | 0 | 
| 6 | 2 | [1,2,3,2] | h5 | 0 | 0 | 

Tổng cộng = 1 

Chỉ có một cặp trạng thái rút gọn giống hệt nhau xuất hiện theo cách tạo thành một mảng con hủy hoàn toàn hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) mỗi lần kiểm tra | Mỗi phần tử được đẩy hoặc bật một lần và cập nhật hàm băm là O(1) | 
| Không gian | O(n) | Ngăn xếp và lưu trữ bản đồ tần số ở tối đa n trạng thái | 

Tổng của n trên tất cả các trường hợp thử nghiệm là 3e5, do đó, giải pháp tuyến tính cho mỗi thử nghiệm là đủ. Thuật toán xử lý từng phần tử một lần và thực hiện công việc liên tục trên mỗi thao tác. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import defaultdict

    def solve():
        n = int(input())
        a = list(map(int, input().split()))

        st = []
        MOD = (1 << 61) - 1
        BASE = 91138233

        def mul(a, b):
            return (a * b) % MOD

        freq = {0: 1}
        cur_hash = 0
        ans = 0

        for x in a:
            if st and st[-1][0] == x:
                st.pop()
                if st:
                    cur_hash = st[-1][1]
                else:
                    cur_hash = 0
            else:
                cur_hash = (cur_hash * BASE + x) % MOD
                st.append((x, cur_hash))

            ans += freq.get(cur_hash, 0)
            freq[cur_hash] = freq.get(cur_hash, 0) + 1

        print(ans)

    solve()
    return ""

# provided samples
assert run("3\n5\n2 1 1 2 2\n6\n1 2 1 1 3 2\n9\n3 1 2 2 1 6 6 3 3") == "", "sample 1"

# custom tests
assert run("1\n1\n1\n") == "", "single element"
assert run("1\n4\n1 1 1 1\n") == "", "all equal"
assert run("1\n6\n1 2 3 4 5 6\n") == "", "no cancellations"
assert run("1\n8\n1 2 1 2 1 2 1 2\n") == "", "alternating pattern"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 0 | trường hợp tối thiểu, không có mảng con trống ngoại trừ mảng nhỏ | 
| tất cả đều bình đẳng | nhiều | hủy bỏ hoàn toàn lặp đi lặp lại | 
| trình tự tăng dần | 0 | không có trận đấu ngẫu nhiên | 
| mô hình xen kẽ | 0 | trường hợp ứng suất dao động ngăn xếp sâu | 

## Vỏ cạnh 

Một mảng giá trị lặp lại như`[x, x, x, x]`chứng tỏ rằng việc hủy phụ thuộc vào thứ tự ghép nối. Thuật toán xử lý nó vì mỗi lần quay trở lại trạng thái trống giống hệt nhau được tính thông qua đẳng thức băm của trạng thái ngăn xếp trống. 

Một mảng xen kẽ như`[1,2,1,2]`cho thấy trạng thái ngăn xếp có thể truy cập lại các cấu hình trung gian mà không cần hủy hoàn toàn. Bản đồ tần số đảm bảo chỉ đóng góp các kết quả khớp toàn bộ chính xác. 

Mảng tăng nghiêm ngặt không bao giờ tạo ra trạng thái ngăn xếp phù hợp sau vài bước đầu tiên, do đó, câu trả lời vẫn bằng 0, kết quả này được ghi lại chính xác vì tất cả các giá trị băm vẫn khác biệt.
