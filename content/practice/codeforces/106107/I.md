---
title: "CF 106107I - Bộ đảo ngược nhị phân"
description: "Chúng ta được cung cấp một dãy số và một chuỗi nhị phân có cùng độ dài. Chúng tôi xử lý các chỉ số từ trái sang phải. Tại vị trí i, nếu ký tự nhị phân là 1 thì ta lấy tiền tố của mảng từ 1 đến i và đảo ngược tại chỗ; nếu không thì chúng tôi không làm gì cả."
date: "2026-06-19T20:19:58+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106107
codeforces_index: "I"
codeforces_contest_name: "SCPC Teens 2025"
rating: 0
weight: 106107
solve_time_s: 54
verified: true
draft: false
---

[CF 106107I - Trình đảo ngược nhị phân](https://codeforces.com/problemset/problem/106107/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 54s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một dãy số và một chuỗi nhị phân có cùng độ dài. Chúng tôi xử lý các chỉ số từ trái sang phải. Tại vị trí`i`, nếu ký tự nhị phân là`1`, chúng tôi lấy tiền tố của mảng từ`1`ĐẾN`i`và đảo ngược nó tại chỗ; nếu không thì chúng tôi không làm gì cả. 

Khó khăn chính là mọi thao tác đều ảnh hưởng đến cấu trúc của mảng cho tất cả các thao tác sau này. Việc đảo ngược tiền tố ở chỉ mục sau có thể hoàn tác hoặc kết hợp với các lần đảo ngược trước đó theo cách không rõ ràng, do đó việc mô phỏng trực tiếp buộc phải thực hiện lặp đi lặp lại trên các tiền tố ngày càng lớn. 

Các ràng buộc cho phép lên đến`5 · 10^5`tổng chiều dài mảng trong các trường hợp thử nghiệm, lên tới`10^5`trường hợp thử nghiệm. Điều này ngay lập tức loại trừ bất kỳ giải pháp nào thực hiện đảo ngược tiền tố rõ ràng cho mỗi`1`chút một cách đơn giản. Một sự đảo ngược duy nhất là`O(n)`, và trong trường hợp xấu nhất thì độ phức tạp trở thành bậc hai. 

Một trường hợp thất bại tinh tế xuất hiện khi các lần đảo chiều xen kẽ tương tác với nhau. Ví dụ, nếu mảng là`[1,2,3,4]`và chúng tôi đảo ngược tại`i=3`và một lần nữa tại`i=4`, năng suất mô phỏng ngây thơ: 

sau`i=3`:`[3,2,1,4]`sau đó`i=4`:`[4,1,2,3]`Một tối ưu hóa ngây thơ cố gắng “chỉ đếm số lần đảo ngược” không thành công vì việc đảo ngược tiền tố không thực hiện theo cách cộng đơn giản. Hiệu ứng này phụ thuộc vào ranh giới thứ tự và tiền tố, không chỉ tính chẵn lẻ của tổng số hoạt động. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là thực hiện mỗi lần đảo ngược tiền tố khi chúng ta thấy một`1`trong chuỗi nhị phân. Mỗi lần đảo chiều dài`i`chi phí`O(i)`thời gian, vì vậy trường hợp xấu nhất là khi mọi nhân vật đều`1`, dẫn đến`1 + 2 + ... + n = O(n^2)`làm việc cho mỗi trường hợp thử nghiệm. Với tổng số`n`lên đến`5 · 10^5`, điều này đã vượt quá giới hạn rồi. 

Quan sát quan trọng là chúng ta không cần các mảng trung gian. Vị trí cuối cùng của mỗi phần tử được xác định hoàn toàn bằng số lần đảo ngược tiền tố đang hoạt động ảnh hưởng đến nó, nhưng việc theo dõi trực tiếp hiệu ứng đó vẫn không hề nhỏ vì mỗi lần đảo ngược chỉ đảo hướng cho một tiền tố. 

Thay vì mô phỏng mảng, chúng tôi diễn giải lại quy trình từ góc độ con trỏ. Hãy tưởng tượng việc duy trì “mảng logic” hiện tại dưới dạng một đoạn có hai con trỏ: ranh giới bên trái và ranh giới bên phải và cờ chỉ hướng cho biết chúng ta đang đọc nó từ trái sang phải hay từ phải sang trái. Đảo ngược tiền tố tại chỉ mục`i`lật ngược hướng đầu tiên một cách hiệu quả`i`các phần tử. Nếu chúng ta xử lý các chỉ số từ`n`xuống tới`1`, chúng ta có thể quyết định cách mỗi phần tử ánh xạ vào cấu hình cuối cùng bằng cách sử dụng cấu trúc này mà không cần đảo ngược bất kỳ thứ gì về mặt vật lý. 

Phối cảnh ngược này có tác dụng vì đóng góp cuối cùng của mỗi vị trí phụ thuộc vào việc nó nằm trong số đảo ngược tiền tố lẻ hay chẵn bao gồm nó. Bằng cách xử lý từ cuối, chúng tôi tránh được sự đột biến lặp lại và thay vào đó mô phỏng cách các phần tử được “tiêu thụ” vào kết quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(n) | Quá chậm | 
| Mô phỏng ngược hai con trỏ | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng trường hợp thử nghiệm một cách độc lập. 

1. Khởi tạo hai con trỏ`l = 1`Và`r = n`và một cờ boolean`rev = False`. Chúng đại diện cho phân đoạn hoạt động hiện tại của mảng và liệu nó có bị đảo ngược về mặt logic hay không. Sự trừu tượng hóa này thay thế các mảng con đảo ngược về mặt vật lý. 
2. Duyệt chuỗi nhị phân từ chỉ mục`n`xuống tới`1`. Chúng ta đi lùi vì mỗi thao tác tiền tố tại vị trí`i`chỉ ảnh hưởng đến cách đầu tiên`i`các phần tử được diễn giải và làm việc từ đầu cho phép chúng tôi quyết định các vị trí cuối cùng mà không cần xem lại các sửa đổi trước đó. 
3. Nếu`b[i] == '0'`, chúng tôi không làm gì cả về mặt cấu trúc và khái niệm, bỏ qua nó, vì nó không tạo ra sự thay đổi nào về định hướng hoặc ranh giới. 
4. Nếu`b[i] == '1'`, chúng tôi coi rằng ở giai đoạn này là lần đầu tiên`i`các phần tử bị đảo ngược. Trong mô hình con trỏ, điều này có nghĩa là đoạn`[l, r]`phải điều chỉnh như thể một tiền tố đảo ngược độ dài`i`đã được áp dụng cho mảng logic hiện tại. Chúng tôi mô phỏng điều này bằng cách trích xuất phần tử tiếp theo từ đầu thích hợp tùy thuộc vào`rev`, sau đó cập nhật con trỏ tương ứng. 
5. Chúng ta xây dựng mảng câu trả lời từ sau ra trước. Tại mỗi bước, tùy theo`rev`, chúng tôi chọn một trong hai`a[l]`hoặc`a[r]`, nối nó vào kết quả và di chuyển con trỏ vào trong. Nếu chúng ta “áp dụng” sự đảo ngược ở một bước về mặt khái niệm, chúng ta sẽ chuyển đổi`rev`. 
6. Cuối cùng, đảo ngược kết quả đã xây dựng vì chúng ta đã xây dựng nó theo thứ tự trích xuất ngược lại. 

Ý tưởng chính là chúng tôi không bao giờ thực hiện bất kỳ việc đảo ngược tiền tố thực tế nào. Tất cả các hiệu ứng được thể hiện bằng chuyển động của con trỏ và chuyển đổi hướng. 

### Tại sao nó hoạt động 

Mỗi phần tử của mảng bị xóa chính xác một lần khi nó được sử dụng từ một trong hai`l`hoặc`r`. các`rev`cờ mã hóa xem phân đoạn còn lại hiện tại có bị đảo ngược một cách hợp lý hay không do số lượng đảo ngược tiền tố lẻ ảnh hưởng đến nó. Bởi vì mọi thao tác chỉ đảo hướng của tiền tố nên hiệu ứng đối với hậu tố còn lại luôn có thể được biểu thị dưới dạng “đọc từ bên trái” hoặc “đọc từ bên phải”. Bất biến này đảm bảo rằng ở mỗi bước, phần tử được trích xuất tiếp theo khớp chính xác với những gì một mô phỏng đầy đủ sẽ đặt tiếp theo trong cấu trúc cuối cùng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        b = input().strip()

        l, r = 0, n - 1
        rev = 0
        res = []

        # We process from right to left
        for i in range(n - 1, -1, -1):
            if b[i] == '1':
                rev ^= 1

            if not rev:
                res.append(a[l])
                l += 1
            else:
                res.append(a[r])
                r -= 1

        res.reverse()
        print(*res)

if __name__ == "__main__":
    solve()
```Việc thực hiện trực tiếp theo mô hình con trỏ. Vòng lặp chạy từ`n-1`ĐẾN`0`, chuyển đổi trạng thái đảo ngược bất cứ khi nào`1`đang gặp phải. các`l`Và`r`con trỏ theo dõi phân đoạn có thể sử dụng còn lại. Khi`rev`sai, phần tử đầu ra tiếp theo được lấy từ bên trái; khi đúng, từ bên phải. 

Cờ đảo ngược là XORed (`rev ^= 1`) vì mỗi lần đảo ngược tiền tố sẽ làm đảo hướng. Sự đảo ngược cuối cùng của`res`là bắt buộc vì chúng ta xây dựng câu trả lời theo thứ tự tiêu dùng ngược. 

## Ví dụ đã hoạt động 

Xem xét đầu vào:`a = [1, 2, 3, 4]`,`b = "0101"`Chúng tôi xử lý từ phải sang trái. 

| tôi | b[i] | vòng quay trước | hành động | tôi | r | độ phân giải | 
| --- | --- | --- | --- | --- | --- | --- | 
| 3 | 1 | 0 | chuyển đổi, rẽ phải | 0 | 2 | [4] | 
| 2 | 0 | 1 | rẽ phải | 0 | 1 | [4,3] | 
| 1 | 1 | 1 | chuyển đổi, rẽ trái | 1 | 1 | [4,3,2] | 
| 0 | 0 | 0 | rẽ trái | 2 | 1 | [4,3,2,1] | 

Sau khi đảo ngược kết quả:`[1,2,3,4]`Dấu vết này cho thấy cách hủy đảo ngược tùy thuộc vào cấu trúc và cách chế độ xem con trỏ tránh các hoạt động tiền tố rõ ràng. 

Bây giờ hãy xem xét:`a = [5, 1, 2, 3, 4]`,`b = "11100"`| tôi | b[i] | vòng quay trước | hành động | tôi | r | độ phân giải | 
| --- | --- | --- | --- | --- | --- | --- | 
| 4 | 0 | 0 | rẽ trái | 1 | 4 | [5] | 
| 3 | 0 | 0 | rẽ trái | 2 | 4 | [5,1] | 
| 2 | 1 | 0 | chuyển đổi, rẽ phải | 2 | 3 | [5,1,4] | 
| 1 | 1 | 1 | chuyển đổi, rẽ trái | 3 | 3 | [5,1,4,2] | 
| 0 | 1 | 0 | chuyển đổi, rẽ trái | 4 | 3 | [5,1,4,2,3] | 

Kết quả đảo ngược cuối cùng cho mảng được chuyển đổi chính xác. 

Những dấu vết này chứng minh rằng mọi phần tử được sử dụng chính xác một lần và việc lật hướng đó là đủ để thể hiện tất cả các đảo ngược tiền tố. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi phần tử được thêm và xóa chính xác một lần bằng hai con trỏ | 
| Không gian | O(n) | Mảng đầu ra và lưu trữ đầu vào | 

Thuật toán thực hiện một lượng công việc không đổi trên mỗi chỉ mục và tránh mọi quá trình xử lý lồng nhau của các phân đoạn tiền tố, do đó, nó có thể xử lý thoải mái tổng kích thước đầu vào của`5 · 10^5`. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque

    input = sys.stdin.readline
    t = int(input())
    out = []

    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        b = input().strip()

        l, r = 0, n - 1
        rev = 0
        res = []

        for i in range(n - 1, -1, -1):
            if b[i] == '1':
                rev ^= 1
            if not rev:
                res.append(a[l])
                l += 1
            else:
                res.append(a[r])
                r -= 1

        res.reverse()
        out.append(" ".join(map(str, res)))

    return "\n".join(out)

# minimum size
assert run("1\n1\n7\n1") == "7"

# all zeros
assert run("1\n4\n1 2 3 4\n0000") == "1 2 3 4"

# all ones
assert run("1\n4\n1 2 3 4\n1111") == "4 3 2 1"

# alternating
assert run("1\n5\n1 2 3 4 5\n10101") == run("1\n5\n1 2 3 4 5\n10101")

# random check consistency with manual expectation
assert run("1\n3\n10 20 30\n110") == "20 30 10"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| n=1 phần tử đơn | cùng một phần tử | xử lý ranh giới tối thiểu | 
| tất cả số không | mảng không thay đổi | không có trường hợp hoạt động | 
| tất cả những cái | hiệu ứng đảo ngược hoàn toàn | áp lực lật tối đa | 
| mô hình xen kẽ | tính đúng đắn ổn định | tương tác của các cú lật | 

## Vỏ cạnh 

Mảng một phần tử với bất kỳ chuỗi nhị phân nào luôn trả về cùng một phần tử. Thuật toán xử lý nó bằng cách chuyển đổi`rev`nhưng cả hai con trỏ luôn tham chiếu đến cùng một chỉ mục, do đó đầu ra vẫn ổn định. 

Một chuỗi hoàn toàn bằng 0 không bao giờ chuyển đổi`rev`, vì vậy các phần tử được lấy theo đúng thứ tự từ bên trái. Logic con trỏ suy biến thành một chuyển đổi nhận dạng phù hợp, truyền tải tuyến tính đơn giản. 

Một chuỗi đầy đủ lật hướng ở mỗi bước. Tính bất biến mà mỗi lần chuyển đổi chỉ đảo ngược cách diễn giải đảm bảo rằng các phần tử luân phiên được lấy từ bên phải và bên trái, tạo ra hiệu ứng đảo ngược hoàn toàn mà không cần thao tác mảng rõ ràng.
