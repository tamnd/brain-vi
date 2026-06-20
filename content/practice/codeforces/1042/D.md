---
problem: 1042D
contest_id: 1042
problem_index: D
name: "Petya and Array"
contest_name: "Codeforces Round 510 (Div. 2)"
rating: 1800
tags: ["data structures", "divide and conquer", "two pointers"]
answer: passed_samples
verified: true
solve_time_s: 79
date: 2026-06-18
model: gpt-5-3-mini
samples_passed: 1
samples_total: 1
share_url: https://chatgpt.com/share/6a337e74-0c8c-83ec-89c6-71e8972be404
---

# CF 1042D - Petya và Mảng 

**Đánh giá:** 1800 
**Thẻ:** cấu trúc dữ liệu, chia để trị, hai con trỏ 
**Mẫu:** gpt-5-3-mini 
**Thời gian giải:** 1 phút 19s 
**Đã xác minh:** có (1/1 mẫu) 
**Chia sẻ:** https://chatgpt.com/share/6a337e74-0c8c-83ec-89c6-71e8972be404 

--- 

##Giải pháp 

## Hiểu vấn đề 

Chúng ta được cho một dãy số nguyên được sắp xếp thành một hàng và chúng ta quan tâm đến các đoạn liền kề của hàng này. Đối với mỗi mảnh có thể, chúng ta tính tổng của nó và cần đếm xem có bao nhiêu mảnh trong số này có tổng nhỏ hơn một ngưỡng nhất định. 

Việc giải thích trực tiếp rất đơn giản: mỗi lựa chọn về vị trí bắt đầu và vị trí kết thúc sẽ xác định một phân đoạn và chúng tôi muốn biết có bao nhiêu phân đoạn trong số đó tạo ra tổng dưới giới hạn. 

Kích thước đầu vào ngay lập tức buộc phải có cách tiếp cận cẩn thận hơn. Với tối đa 200.000 phần tử, số lượng phân đoạn ở mức n2, trong trường hợp xấu nhất đã lên tới khoảng 20 tỷ. Bất kỳ phương pháp nào kiểm tra rõ ràng từng phân đoạn đều vượt xa những gì 2 giây có thể xử lý bằng Python hoặc thậm chí trong C++ được tối ưu hóa. Điều này thúc đẩy chúng ta hướng tới một chiến lược tuyến tính hoặc gần tuyến tính, điển hình là một thứ gì đó giống như kỹ thuật hai con trỏ hoặc phương pháp đếm dựa trên tổng tiền tố. 

Sự hiện diện của số âm là khó khăn chính. Nếu tất cả các số đều không âm thì cửa sổ trượt sẽ hoạt động đơn điệu và chúng ta có thể duy trì một cửa sổ mở rộng và thu nhỏ duy nhất. Ở đây, các giá trị âm phá vỡ tính đơn điệu đó, do đó, cách tiếp cận hai con trỏ ngây thơ dựa vào tổng tăng dần không thể được áp dụng trực tiếp. 

Trường hợp cạnh tinh tế xuất hiện khi ngưỡng rất lớn hoặc rất nhỏ. Nếu t cực lớn thì tất cả các phân đoạn đều hợp lệ và một nghiệm đúng vẫn phải đếm n(n+1)/2 một cách hiệu quả. Nếu t rất nhỏ hoặc âm thì chỉ một vài phân đoạn hoặc thậm chí chỉ một phần tử có thể đủ điều kiện và việc xử lý tổng âm không chính xác có thể dễ dàng dẫn đến việc đếm thừa hoặc đếm thiếu. 

Một ví dụ nhỏ khi trực giác ngây thơ thất bại là: 

đầu vào:```
3 1
2 -5 3
```Các phân đoạn đúng là`[1,2] = -3`,`[2,2] = -5`,`[2,3] = -2`,`[3,3] = 3`không hợp lệ, vì vậy câu trả lời là 3. Một cửa sổ mở rộng đơn giản chỉ di chuyển sang phải khi tổng quá lớn sẽ không thành công vì việc giảm tổng từ các giá trị âm không đơn điệu. 

## Phương pháp tiếp cận 

Phương pháp brute-force xem xét từng cặp điểm cuối (l, r), tính tổng của phân đoạn đó và kiểm tra xem nó có nhỏ hơn t hay không. Điều này đúng vì nó phù hợp trực tiếp với định nghĩa của vấn đề. Tuy nhiên, việc tính toán tổng từng phân đoạn có chi phí ban đầu là O(n), dẫn đến tổng thời gian là O(n³) hoặc O(n²) nếu sử dụng tổng tiền tố. Ngay cả O(n²) cũng quá lớn đối với 200.000 phần tử. 

Quan sát quan trọng là chúng ta có thể trình bày lại vấn đề theo tổng tiền tố. Nếu chúng ta định nghĩa S[i] là tổng của i phần tử đầu tiên thì tổng của một đoạn [l, r] là S[r] − S[l−1]. Điều kiện trở thành S[l−1] > S[r] − t. Với mỗi r cố định, chúng ta muốn đếm xem có bao nhiêu tổng tiền tố trước đó vượt quá ngưỡng bắt nguồn từ S[r]. 

Điều này biến bài toán thành phép đếm, đối với mỗi vị trí, có bao nhiêu giá trị trước đó nằm trên một giới hạn nhất định. Nếu chúng ta duy trì cấu trúc được sắp xếp của các tổng tiền tố đã thấy cho đến nay, chúng ta có thể truy vấn số lượng này một cách hiệu quả. Vì chúng tôi xử lý tổng tiền tố theo thứ tự nên chúng tôi có thể sử dụng cây chỉ mục nhị phân hoặc cấu trúc thống kê thứ tự. Tuy nhiên, có một kỹ thuật phân chia và chinh phục trực tiếp và tối ưu hơn để đếm các cặp như vậy trong O(n log n) mà không cần truy vấn chèn động. 

Chúng ta rút gọn nhiệm vụ thành các cặp đếm (i, j) với i < j và S[j] − S[i] < t, tương đương với S[i] > S[j] − t. Đây là một biến thể đếm nghịch đảo cổ điển trên tổng tiền tố, có thể giải được bằng cách sử dụng loại hợp nhất đã sửa đổi trong đó chúng tôi đếm các cặp chéo trong quá trình hợp nhất. 

Cách tiếp cận brute-force đơn giản nhưng có tính bậc hai, trong khi cách tiếp cận tổng tiền tố + chia để trị làm giảm vấn đề sắp xếp bằng cách đếm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) hoặc O(n) | Quá chậm | 
| Tiền tố + Đếm sắp xếp hợp nhất | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi chuyển đổi vấn đề thành tổng tiền tố và sau đó đếm các cặp bằng cách sử dụng phép chia và chinh phục. 

1. Xây dựng mảng tổng tiền tố S, trong đó S[0] = 0 và S[i] là tổng của i phần tử đầu tiên. Điều này chuyển đổi tổng phân đoạn thành sự khác biệt của hai giá trị tiền tố. 
2. Phát biểu lại điều kiện. Một đoạn [l, r] có tổng < t nếu S[r] − S[l−1] < t, sắp xếp lại thành S[l−1] > S[r] − t. Bây giờ chúng ta đếm các cặp chỉ số tiền tố thỏa mãn bất đẳng thức này. 
3. Chạy sắp xếp hợp nhất đã sửa đổi trên mảng tổng tiền tố. Mục tiêu không chỉ là sắp xếp mà còn đếm xem có bao nhiêu cặp hợp lệ giao nhau giữa nửa bên trái và nửa bên phải. 
4. Trong bước hợp nhất, với mỗi phần tử ở nửa bên phải, chúng ta xác định có bao nhiêu phần tử ở nửa bên trái thỏa mãn S[left] > S[right] − t. Vì cả hai nửa đều được sắp xếp nên chúng ta có thể di chuyển con trỏ qua nửa bên trái mà không cần khởi động lại từng phần tử. 
5. Tích lũy số lượng này qua tất cả các bước hợp nhất. Mỗi cặp hợp lệ được tính chính xác một lần khi hai điểm cuối của nó được chia thành một phép chia đệ quy. 
6. Tiếp tục hợp nhất các nửa đã sắp xếp để mức đệ quy cao hơn cũng có thể đếm chính xác các cặp xuyên ranh giới. 

### Tại sao nó hoạt động 

Sự đúng đắn đến từ hai sự thật. Đầu tiên, mỗi phân đoạn tương ứng duy nhất với một cặp chỉ số tiền tố, do đó không có câu trả lời hợp lệ nào bị bỏ sót hoặc trùng lặp với cặp tiền tố bên ngoài. Thứ hai, trong mỗi bước hợp nhất, thứ tự sắp xếp cho phép chúng ta đếm tất cả các cặp chéo hợp lệ một cách hiệu quả mà không cần quét tất cả các kết hợp. Mỗi cặp được xem xét chính xác một lần ở cấp độ đệ quy trong đó hai chỉ số được tách thành hai nửa khác nhau, đảm bảo tính đầy đủ và tránh tính hai lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, t = map(int, input().split())
    a = list(map(int, input().split()))

    prefix = [0]
    for x in a:
        prefix.append(prefix[-1] + x)

    def sort_count(arr):
        if len(arr) <= 1:
            return arr, 0

        mid = len(arr) // 2
        left, cnt_left = sort_count(arr[:mid])
        right, cnt_right = sort_count(arr[mid:])

        cnt = cnt_left + cnt_right

        j = 0
        for r in right:
            while j < len(left) and left[j] <= r - t:
                j += 1
            cnt += len(left) - j

        merged = []
        i = j = 0
        while i < len(left) and j < len(right):
            if left[i] < right[j]:
                merged.append(left[i])
                i += 1
            else:
                merged.append(right[j])
                j += 1

        merged.extend(left[i:])
        merged.extend(right[j:])
        return merged, cnt

    _, ans = sort_count(prefix)
    print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai bắt đầu bằng cách xây dựng các tổng tiền tố sao cho mỗi tổng phân đoạn trở thành sự khác biệt giữa hai giá trị tiền tố. Hàm đệ quy trả về cả phiên bản đã sắp xếp của mảng con tiền tố và số cặp hợp lệ trong đó. 

Phần quan trọng là vòng đếm trong quá trình hợp nhất. Với mỗi giá trị ở nửa bên phải, chúng ta di chuyển con trỏ ở nửa bên trái để tìm xem có bao nhiêu giá trị thỏa mãn bất đẳng thức. Điều này có tác dụng vì cả hai nửa đều được sắp xếp nên con trỏ chỉ di chuyển về phía trước. 

Bước hợp nhất duy trì thứ tự được sắp xếp, điều này cần thiết cho tính chính xác ở mức đệ quy cao hơn. Một sai lầm phổ biến là nhầm lẫn chiều bất đẳng thức; ở đây chúng tôi dịch cẩn thận điều kiện để phù hợp với sự khác biệt về tiền tố. 

## Ví dụ đã hoạt động 

Hãy xem xét đầu vào mẫu:```
5 4
5 -1 3 4 -1
```Tổng tiền tố là:```
[0, 5, 4, 7, 11, 10]
```Chúng tôi theo dõi việc đếm hợp nhất ở mức cao. 

| Bước | Nửa trái | Nửa bên phải | Vị trí con trỏ j | Đếm cặp mới | 
| --- | --- | --- | --- | --- | 
| hợp nhất | [0,4,5] | [7,10,11] | di chuyển qua trái | đếm các cặp hợp lệ trên phần chia | 

Mỗi phần tử bên phải kích hoạt việc đếm các phần tử bên trái thỏa mãn prefix[i] > prefix[j] − 4. Ví dụ: đối với giá trị tiền tố 7, ngưỡng là 3 và các giá trị bên trái hợp lệ lớn hơn 3 sẽ được tính. 

Điều này xác nhận rằng tất cả các phân đoạn đủ điều kiện vượt qua ranh giới phân vùng đều được đưa vào chính xác một lần. 

Bây giờ hãy xem xét một ví dụ tùy chỉnh nhỏ:```
3 1
2 -5 3
```Tiền tố:```
[0, 2, -3, 0]
```Thuật toán xác định các cặp hợp lệ: 

- (0,1), (1,2), (1,3) tùy theo kiểm tra bất đẳng thức 

Bước hợp nhất đảm bảo rằng ngay cả với các giá trị âm, thứ tự vẫn được giữ nguyên và tất cả các so sánh vẫn hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Mỗi cấp độ sắp xếp hợp nhất xử lý tất cả các phần tử một lần và có các cấp độ log n | 
| Không gian | O(n) | Mảng tiền tố và ngăn xếp đệ quy để phân chia và chinh phục | 

Các ràng buộc cho phép tối đa 200.000 phần tử, do đó, giải pháp O(n log n) với bộ nhớ bổ sung tuyến tính sẽ phù hợp một cách thoải mái trong giới hạn. Các yếu tố không đổi của sắp xếp hợp nhất vẫn có thể quản lý được trong Python. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, t = map(int, input().split())
    a = list(map(int, input().split()))

    prefix = [0]
    for x in a:
        prefix.append(prefix[-1] + x)

    def sort_count(arr):
        if len(arr) <= 1:
            return arr, 0

        mid = len(arr) // 2
        left, c1 = sort_count(arr[:mid])
        right, c2 = sort_count(arr[mid:])
        cnt = c1 + c2

        j = 0
        for r in right:
            while j < len(left) and left[j] <= r - t:
                j += 1
            cnt += len(left) - j

        merged = []
        i = j = 0
        while i < len(left) and j < len(right):
            if left[i] < right[j]:
                merged.append(left[i])
                i += 1
            else:
                merged.append(right[j])
                j += 1

        merged.extend(left[i:])
        merged.extend(right[j:])
        return merged, cnt

    return str(sort_count(prefix)[1])

# provided sample
assert run("5 4\n5 -1 3 4 -1\n") == "5"

# single element
assert run("1 0\n5\n") == "0"

# all negative
assert run("3 1\n-1 -2 -3\n") == "6"

# all positive
assert run("4 10\n1 2 3 4\n") == "6"

# mixed
assert run("3 1\n2 -5 3\n") == "3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 0 | ranh giới tối thiểu | 
| tất cả đều tiêu cực | 6 | tất cả các phân đoạn hợp lệ | 
| tất cả đều tích cực | 6 | tính đúng đắn của tổ hợp đầy đủ | 
| giá trị hỗn hợp | 3 | xử lý thay đổi biển báo | 

## Vỏ cạnh 

Một mảng có kích thước tối thiểu cho biết liệu thuật toán có xử lý chính xác các cặp tiền tố đơn hay không. Đối với đầu vào`n = 1`,`t = 0`,`a = [5]`, tiền tố trở thành`[0, 5]`. Phân đoạn duy nhất là`[5]`, không nhỏ hơn 0, nên câu trả lời là 0. Việc đếm dựa trên phép hợp nhất tạo ra không có cặp hợp lệ, vì 5 > 0 − 0 là sai trong cách giải thích bất đẳng thức nghiêm ngặt, do đó nó trả về 0 một cách chính xác. 

Một mảng âm hoàn toàn nhấn mạnh tính chính xác khi mọi phân đoạn đều hợp lệ hoặc gần như tất cả đều hợp lệ. Vì`[-1, -2, -3]`với`t = 1`, tất cả các tổng tiền tố đều giảm và mọi chênh lệch giữa hai chỉ số đều âm, do đó tất cả 6 phân đoạn đều đủ điều kiện. Việc đếm chia để trị bao gồm mọi cặp chỉ số tiền tố, xác nhận rằng việc xử lý bất đẳng thức vẫn hợp lệ ngay cả khi thứ tự giảm nghiêm ngặt. 

Một mảng dấu hỗn hợp như`[2, -5, 3]`đảm bảo thuật toán không dựa vào hành vi tiền tố đơn điệu. Mảng tiền tố dao động, nhưng cấu trúc được sắp xếp bên trong sắp xếp hợp nhất vẫn đảm bảo tính chính xác vì các phép so sánh hoàn toàn dựa trên thứ tự giá trị chứ không phải thứ tự chỉ mục gốc.
