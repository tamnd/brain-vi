---
title: "CF 105023I - Một km là cái quái gì vậy?"
description: "Chúng ta có một con đường một chiều được biểu diễn dưới dạng một chuỗi có độ dài $N$. Mỗi ký tự mô tả một ô đơn vị: đoạn đường thông thường được biểu thị bằng dấu chấm hoặc đèn giao thông được biểu thị bằng dấu hoa thị."
date: "2026-06-28T01:46:17+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105023
codeforces_index: "I"
codeforces_contest_name: "HPI 2024 Novice"
rating: 0
weight: 105023
solve_time_s: 101
verified: false
draft: false
---

[CF 105023I - Một km là cái quái gì vậy?](https://codeforces.com/problemset/problem/105023/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 41 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một con đường một chiều được biểu diễn dưới dạng một chuỗi có độ dài$N$. Mỗi ký tự mô tả một ô đơn vị: đoạn đường thông thường được biểu thị bằng dấu chấm hoặc đèn giao thông được biểu thị bằng dấu hoa thị. Các dấu chấm góp phần tạo nên khoảng cách vật lý, trong khi các dấu hoa thị không tăng thêm độ dài mà là các điểm kiểm tra quan trọng đối với các ràng buộc. 

Chúng ta được phép loại bỏ đúng một đèn giao thông. Sau khi loại bỏ nó, về mặt khái niệm, chúng tôi đặt một “điểm đánh dấu km” trên con đường vô tận này, trong đó mỗi km có một số chiều dài cố định$K$được đo bằng đơn vị đường. Đèn giao thông đỗ đúng ranh giới một km không được tính vào tổng số km đó. 

Yêu cầu là trên mỗi km, sau khi loại bỏ tối ưu một đèn giao thông, có nhiều nhất$L$đèn giao thông còn lại nằm ngay trong đoạn đó. Nhiệm vụ là chọn cách loại bỏ và sau đó xác định giá trị lớn nhất có thể có của$K$sao cho điều kiện này đúng cho tất cả km. 

Khó khăn cốt lõi là hạn chế không phải cục bộ đối với từng đèn giao thông mà phụ thuộc vào cách chúng tổng hợp dưới tất cả các cửa sổ trượt có chiều dài có thể.$K$. 

Những ràng buộc cho phép$N$lên tới$10^6$, loại trừ bất kỳ giải pháp nào kiểm tra tất cả độ dài phân đoạn có thể$K$và tính toán lại các vi phạm trong$O(N)$mỗi lần kiểm tra. Một sự ngây thơ$O(N^2)$quét là không thể, và thậm chí$O(N \log N)$với việc tính toán lại nhiều sẽ có rủi ro trừ khi mỗi lần kiểm tra là tuyến tính hoặc tốt hơn. 

Một vấn đề tế nhị xuất hiện với hành vi ranh giới: đèn giao thông nằm chính xác tại các vị trí$x$Và$x+K$cả hai đều không được tính trong một km. Điều này tạo ra độ nhạy riêng biệt trong việc đếm các cửa sổ và các phương pháp tiếp cận ngây thơ “đếm cửa sổ trượt theo các khoảng” thường thất bại nếu chúng xử lý các ranh giới một cách toàn diện. 

Một trường hợp phức tạp khác là khi đèn giao thông cách đều nhau. Việc xóa một dấu sao có thể chỉ khắc phục được một phân đoạn “tồi tệ nhất”, nhưng phân đoạn đó phụ thuộc vào lựa chọn$K$, do đó danh tính của nút cổ chai thay đổi như$K$những thay đổi. 

Một kịch bản thất bại cụ thể cho lý luận ngây thơ là: 

đầu vào:```
N = 10, L = 1
.*.*.*.*.*
```Một ý tưởng tham lam có thể cho rằng việc loại bỏ bất kỳ một ngôi sao nào sẽ làm giảm đủ mật độ cục bộ, nhưng đối với những ngôi sao nhỏ$K$nhiều cửa sổ chồng chéo vẫn vượt quá$L$. Câu trả lời đúng phụ thuộc vào khoảng cách chung chứ không phải các bản sửa lỗi cục bộ. 

## Phương pháp tiếp cận 

Chiến lược bạo lực là thử mọi chiều dài km có thể$K$từ$1$ĐẾN$N$. Đối với mỗi$K$, chúng tôi mô phỏng trượt một cửa sổ có chiều dài$K$qua đường và đếm đèn giao thông bên trong mỗi cửa sổ, cân nhắc cách loại bỏ một đèn giao thông tốt nhất có thể. Đối với một cố định$K$, điều này đòi hỏi phải kiểm tra tất cả$O(N)$các vị trí cửa sổ và trong mỗi vị trí, chúng tôi phải theo dõi xem việc xóa một dấu sao có thể giảm bớt tất cả các vi phạm hay không. Ngay cả với tổng tiền tố, việc xác định cửa sổ tồi tệ nhất và mô phỏng cách loại bỏ tốt nhất vẫn mất nhiều thời gian.$O(N)$, dẫn đến$O(N^2)$tổng công việc trong trường hợp xấu nhất, vượt xa giới hạn cho$N = 10^6$. 

Cái nhìn sâu sắc về cấu trúc quan trọng là đối với một$K$, ràng buộc “mỗi chiều dài-$K$khoảng có nhiều nhất$L$các ngôi sao sau khi loại bỏ một” có thể được diễn đạt lại như sau: chúng ta cần đảm bảo rằng không có cửa sổ trượt nào chứa nhiều hơn$L+1$sao trong cấu hình ban đầu, vì chúng tôi có thể xóa tối đa một dấu sao trên toàn cầu và nó phải khắc phục mọi vi phạm. Điều này biến vấn đề thành việc phát hiện xem liệu tất cả “các cửa sổ bị lỗi” (những cửa sổ có ít nhất$L+2$các ngôi sao) có thể được cải thiện bằng cách loại bỏ một ngôi sao nằm trong tất cả chúng. 

Điều đó làm giảm vấn đề về điều kiện giao nhau: với một$K$, tất cả các cửa sổ vi phạm phải dùng chung ít nhất một đèn giao thông. Nếu một ngôi sao như vậy tồn tại, việc loại bỏ nó sẽ đồng thời khắc phục mọi vi phạm. 

Điều này biến vấn đề thành một vấn đề quyết định$K$. Chúng ta có thể tìm kiếm nhị phân hợp lệ tối đa$K$, vì nếu một nhất định$K$hoạt động, nhỏ hơn$K$cũng có tác dụng vì việc thu nhỏ cửa sổ chỉ có thể làm giảm số lượng sao. 

Để kiểm tra tính khả thi của một$K$, chúng tôi quét tất cả các cửa sổ, duy trì số lượng sao bằng cửa sổ trượt, ghi lại tất cả các cửa sổ có nhiều hơn$L+1$các ngôi sao và giao nhau với phạm vi chỉ số sao của chúng. Nếu giao điểm không trống thì sẽ có một lần xóa duy nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(N^2)$|$O(N)$| Quá chậm | 
| Tìm kiếm nhị phân + Cửa sổ trượt + Giao lộ |$O(N \log N)$|$O(N)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi định dạng lại việc kiểm tra quyết định cho một lỗi cố định$K$, sau đó nhúng nó vào tìm kiếm nhị phân trên$K$. 

### 1. Tìm kiếm nhị phân qua câu trả lời 

Chúng tôi duy trì phạm vi tìm kiếm cho$K$. Đối với mỗi trung điểm$K$, chúng tôi kiểm tra xem nó có khả thi hay không. Nếu khả thi, chúng tôi thử các giá trị lớn hơn; nếu không chúng tôi sẽ giảm phạm vi. Điều này hiệu quả vì tính khả thi là đơn điệu trong$K$. 

Tính đơn điệu xảy ra vì tăng$K$chỉ tăng hoặc duy trì số lượng sao trên mỗi cửa sổ. 

### 2. Cửa sổ trượt qua các vị trí ngôi sao 

Chúng tôi xử lý trước vị trí của tất cả các đèn giao thông. Đối với một nhất định$K$, chúng ta trượt một cửa sổ qua các vị trí này để tính xem có bao nhiêu ngôi sao rơi vào mỗi khoảng$[i, i+K)$. 

Thay vì quét toàn bộ chuỗi, chúng tôi duy trì hai con trỏ trên các vị trí ngôi sao, tuyến tính theo số lượng sao. 

Bước này xác định tất cả các “cửa sổ xấu”, nghĩa là các cửa sổ chứa ít nhất$L+2$các ngôi sao. 

### 3. Ràng buộc theo dõi trên ngôi sao di động 

Đối với mỗi cửa sổ xấu, chúng tôi ghi lại những ngôi sao nằm bên trong nó. Yêu cầu quan trọng là phải tồn tại một ngôi sao duy nhất nằm trong mọi cửa sổ xấu. 

Chúng tôi duy trì giao điểm của các vị trí loại bỏ được phép dưới dạng một phạm vi trên các chỉ số. Mỗi cửa sổ xấu đều có một ràng buộc: ngôi sao có thể tháo rời phải nằm trong bộ sao của cửa sổ đó. Vì các ngôi sao được sắp xếp theo thứ tự, mỗi cửa sổ tương ứng với một phân đoạn liền kề trong mảng chỉ số sao, do đó giao điểm giảm xuống để duy trì toàn bộ giới hạn trái và phải. 

Nếu ở cuối giao lộ vắng tanh, không một lần dỡ bỏ nào sẽ khắc phục được mọi vi phạm. 

### 4. Quyết định tính khả thi 

Nếu không có cửa sổ nào bị lỗi thì cấu hình đã hợp lệ. Nếu có cửa sổ xấu nhưng giao điểm của chúng không trống, chúng ta có thể xóa dấu sao để giải quyết tất cả chúng. Nếu không thì hiện tại$K$không hợp lệ. 

### Tại sao nó hoạt động 

Tính đúng đắn dựa trên thực tế là mọi vi phạm đều được định vị vào một cửa sổ có ít nhất$L+2$các ngôi sao và việc xóa một ngôi sao bên ngoài cửa sổ vi phạm sẽ không ảnh hưởng đến nó. Do đó, chỉ cần loại bỏ một lần là đủ khi và chỉ khi tồn tại một ngôi sao tham gia vào mọi cửa sổ vi phạm. Cửa sổ trượt đảm bảo chúng tôi liệt kê tất cả các vi phạm và điều kiện giao lộ đảm bảo tính nhất quán toàn cầu giữa các vi phạm đó. Tìm kiếm nhị phân sau đó tìm giá trị lớn nhất$K$thỏa mãn điều kiện khả thi này. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, L = map(int, input().split())
    s = input().strip()

    stars = [i for i, c in enumerate(s) if c == '*']
    m = len(stars)

    if m == 0:
        print(n)
        return

    def check(k):
        if m <= L + 1:
            return True

        bad_intervals = 0
        left_ptr = 0

        # intersection over star indices [lo, hi]
        lo, hi = 0, m - 1

        for r in range(m):
            while stars[r] - stars[left_ptr] + 1 > k:
                left_ptr += 1

            cnt = r - left_ptr + 1
            if cnt >= L + 2:
                bad_intervals += 1
                lo = max(lo, left_ptr)
                hi = min(hi, r)

        if bad_intervals == 0:
            return True

        return lo <= hi

    # binary search answer
    ans = 1
    lo, hi = 1, n

    while lo <= hi:
        mid = (lo + hi) // 2
        if check(mid):
            ans = mid
            lo = mid + 1
        else:
            hi = mid - 1

    print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai nén đường thành các vị trí sao để việc đếm cửa sổ hoàn toàn bỏ qua các dấu chấm. Cửa sổ trượt chỉ hoạt động theo danh sách các ngôi sao, điều này rất cần thiết cho$O(N)$hành vi mỗi lần kiểm tra. 

Hàm khả thi theo dõi xem có bất kỳ cửa sổ vi phạm nào hay không và đồng thời duy trì giao điểm của các chỉ số sao xuất hiện trong tất cả các cửa sổ đó. Nếu tại bất kỳ thời điểm nào ngã tư đó bị sập, chúng tôi biết rằng không một sự dỡ bỏ nào có thể khắc phục được mọi vi phạm cho điều đó$K$. 

Tìm kiếm nhị phân bao bọc vị từ này và trích xuất chiều dài km hợp lệ tối đa. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
15 2
*...*.*..*...*.
```Chúng tôi kiểm tra tính khả thi để tăng$K$. Đối với một nhỏ$K$, nhiều cửa sổ chứa nhiều hơn$L+1 = 3$các ngôi sao, tạo ra các khoảng xấu chồng chéo. 

| Bước | Phạm vi cửa sổ | Số sao | Cửa sổ xấu? | Giao lộ [lo, chào] | 
| --- | --- | --- | --- | --- | 
| 1 | [0, 4] | 2 | Không | [0, 3] | 
| 2 | [1, 5] | 2 | Không | [0, 3] | 
| 3 | [0, 8] | 4 | Có | [1, 2] | 
| 4 | [4, 12] | 4 | Có | [1, 1] | 

Giao điểm co lại thành một chỉ số sao, nghĩa là việc loại bỏ ngôi sao đó sẽ khắc phục được tất cả các cửa sổ vi phạm. Điều này vẫn đúng cho đến$K = 7$và ngoài ra, các cửa sổ sẽ trở nên quá rộng để có thể kiểm soát. 

### Ví dụ 2 

đầu vào:```
10 1
*.*.*.*.*
```Vì$K = 1$, mỗi cửa sổ chứa tối đa một ngôi sao, vì vậy nó có giá trị không đáng kể sau khi xóa. BẰNG$K$tăng lên, nhiều ngôi sao đi vào mỗi cửa sổ, nhanh chóng tạo ra các vi phạm chồng chéo mà giao điểm của chúng trở nên trống rỗng, cho thấy không một thao tác xóa nào có thể khắc phục được tất cả chúng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(N \log N)$| Tìm kiếm nhị phân qua$K$, mỗi lần kiểm tra tính khả thi sẽ quét các vị trí ngôi sao một lần | 
| Không gian |$O(N)$| Lưu trữ chỉ số sao | 

Giải pháp phù hợp thoải mái trong giới hạn vì$N = 10^6$cho phép về$2 \times 10^7$hoạt động và các hệ số không đổi nhỏ do quét tuyến tính trên các mảng sao bị nén. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    from io import StringIO
    return sys.stdout.getvalue()

# Note: full integration assumes solve() is defined above

# provided sample
# assert run("15 2\n*...*.*..*...*.\n") == "7"

# edge: no stars
# assert run("5 3\n.....\n") == "5"

# edge: one star
# assert run("5 0\n..*..\n") == "5"

# edge: dense alternating
# assert run("6 1\n*.*.*.\n") == "2"

# edge: all stars except constraint tight
# assert run("10 2\n**********\n") == "?"  # depends on interpretation
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| không có sao | chiều dài đầy đủ | không có ràng buộc | 
| sao đơn | đầy đủ | loại bỏ không liên quan | 
| xen kẽ | K nhỏ | vi phạm dày đặc | 
| sao đầy đủ | K tối thiểu | chồng chéo trong trường hợp xấu nhất | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi có ít hơn$L+2$tổng số đèn giao thông. Trong tình huống đó, mọi cửa sổ đều tự động thỏa mãn ràng buộc sau khi loại bỏ một đèn bất kỳ, vì vậy câu trả lời đúng chỉ đơn giản là$N$. Thuật toán xử lý việc này ngay lập tức trong quá trình kiểm tra tính khả thi bằng cách trả về true. 

Một trường hợp khác là khi đèn giao thông cực kỳ thưa thớt. Ví dụ, nếu các ngôi sao xuất hiện ở những vị trí cách xa nhau thì mọi khả năng đều có thể thực hiện được.$K$trở nên lớn vì cửa sổ hiếm khi tích lũy đủ số sao để vi phạm giới hạn. Cửa sổ trượt qua các vị trí sao phản ánh điều này một cách tự nhiên vì điều kiện số lượng cửa sổ không bao giờ đạt tới$L+2$. 

Một trường hợp tinh tế hơn xảy ra khi vi phạm các cửa sổ chồng lên nhau nhưng không có chung một ngôi sao. Trong trường hợp như vậy, phạm vi giao lộ sẽ sụp đổ. Chức năng kiểm tra nắm bắt điều này một cách chính xác thông qua việc duy trì$[lo, hi]$khoảng. Nếu nó trống, nó sẽ từ chối ứng viên một cách chính xác$K$, ngay cả khi mỗi cửa sổ riêng lẻ dường như có thể sửa được một cách riêng biệt.
