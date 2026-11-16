---
title: "CF 1913C - Trò chơi với nhiều bộ"
description: "Chúng tôi đang duy trì một tập hợp sức mạnh năng động của hai người. Mỗi lần chúng tôi nhận được một thao tác THÊM, chúng tôi sẽ chèn một giá trị có dạng $2^x$ vào nhiều tập hợp."
date: "2026-06-08T20:07:41+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "bitmasks", "brute-force", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1913
codeforces_index: "C"
codeforces_contest_name: "Educational Codeforces Round 160 (Rated for Div. 2)"
rating: 1300
weight: 1913
solve_time_s: 87
verified: true
draft: false
---

[CF 1913C - Trò chơi với nhiều bộ](https://codeforces.com/problemset/problem/1913/C) 

**Đánh giá:** 1300 
**Tags:** tìm kiếm nhị phân, bitmask, vũ phu, tham lam 
**Thời gian giải:** 1 phút 27s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang duy trì một tập hợp sức mạnh năng động của hai người. Mỗi lần chúng tôi nhận được một thao tác THÊM, chúng tôi sẽ chèn một giá trị có dạng$2^x$vào nhiều tập hợp. Mỗi thao tác GET đặt ra một câu hỏi về khả năng tiếp cận: liệu chúng ta có thể chọn một số tập hợp con của các số hiện có có tổng bằng mục tiêu không$w$. 

Đặc điểm chính là mọi phần tử đều là lũy thừa thuần túy của hai và được phép trùng lặp vì nhiều tập hợp có thể chứa các phần chèn lặp lại có cùng lũy ​​thừa. Điều đó biến cấu trúc thành một thứ gì đó gần giống với hệ thống biểu diễn nhị phân, nhưng có một điểm khó khăn là chúng ta có thể có nhiều “đồng xu” có cùng giá trị. 

Các ràng buộc buộc chúng tôi phải xử lý tới$10^5$hoạt động trực tuyến. Mỗi truy vấn GET có thể liên quan đến một mục tiêu lên tới$10^9$, vì vậy chúng tôi không thể thử tất cả các tập hợp con hoặc thậm chí thực hiện lý luận hàm mũ cho mỗi truy vấn. Bất kỳ giải pháp nào cố gắng liệt kê các tập hợp con hoặc chạy DP kiểu ba lô cho mỗi truy vấn sẽ ngay lập tức thất bại, vì$2^{30}$sự kết hợp tập hợp con cho mỗi truy vấn đã vượt quá giới hạn. 

Một vấn đề tế nhị xuất hiện khi nhiều sức mạnh giống hệt nhau tích lũy. Ví dụ: nếu chúng ta chèn ba$2^0$các phần tử, chúng ta có thể tính các tổng như 1, 2 hoặc 3 chỉ bằng cách sử dụng các phần tử đó. Một cách tiếp cận ngây thơ xử lý từng quyền lực một cách độc lập mà không tính đến số lượng sẽ thất bại ở đây. Giải thích đúng là mỗi số mũ hoạt động giống như một số giới hạn của các đồng tiền giống hệt nhau và việc mang giữa các bit trở nên khả thi. 

Một trường hợp thất bại khác là xử lý từng GET một cách độc lập bằng một phân rã tham lam mới nhưng lại quên rằng các hoạt động ADD trước đó đã tích lũy trạng thái. Vì nhiều tập hợp phát triển nên việc tính toán lại từ đầu cho mỗi truy vấn cũng sẽ quá chậm. 

## Phương pháp tiếp cận 

Việc giải thích brute-force rất đơn giản: đối với mỗi truy vấn GET, hãy xem xét tất cả các tập hợp con của nhiều tập hợp và kiểm tra xem có tập hợp con nào tính tổng bằng$w$. Nếu có$k$các phần tử hiện có trong multiset, điều này đòi hỏi phải kiểm tra$2^k$tập hợp con, tăng theo cấp số nhân. Ngay cả đối với$k = 40$, điều này trở nên không khả thi, và ở đây$k$có thể đạt được$10^5$. 

Cải tiến đơn giản thứ hai là duy trì tất cả các tổng tập hợp con thông qua tập lập trình động. Sau mỗi lần THÊM, chúng tôi hợp nhất các tổng mới bằng cách cộng lũy ​​thừa mới cho tất cả các tổng hiện có. Điều này vẫn nhân đôi số tiền có thể truy cập trên mỗi lần chèn trong trường hợp xấu nhất, một lần nữa dẫn đến sự tăng trưởng theo cấp số nhân. 

Quan sát quan trọng là tất cả các giá trị đều là lũy thừa của hai. Điều đó có nghĩa là mỗi số đóng góp vào chính xác một vị trí nhị phân và việc kết hợp chúng hoạt động giống như phép cộng nhị phân với khả năng mang. Thay vì theo dõi các tập hợp con, chúng ta chỉ cần theo dõi xem có bao nhiêu bản sao của mỗi số mũ tồn tại. 

Cho phép$c[i]$là số phần tử bằng$2^i$. Để trả lời liệu chúng ta có thể hình thành$w$, chúng tôi cố gắng giải thích$w$ở dạng nhị phân và khớp các bit một cách tham lam từ thấp nhất đến cao nhất, sử dụng số lượng có sẵn và mang số dư lên trên. Điều này làm giảm vấn đề xuống một hệ thống đồng xu giới hạn trên các trọng số nhị phân, trong đó việc truyền bá tham lam là hợp lệ vì mỗi trọng số chính xác gấp đôi trọng số trước đó. 

Điều này biến mỗi truy vấn GET thành một vòng lặp nhỏ với tối đa 31 bit. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(2^n)$mỗi truy vấn |$O(n)$| Quá chậm | 
| Tối ưu |$O(30)$mỗi truy vấn |$O(30)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì một mảng`cnt`Ở đâu`cnt[i]`lưu trữ bao nhiêu lần$2^i$đã được chèn vào. 

1. Để THÊM$x$, tăng`cnt[x]`bởi một. Điều này trực tiếp ghi lại sự sẵn có của sức mạnh đó. 
2. Để NHẬN$w$, mô phỏng tòa nhà$w$từng chút một từ mức công suất thấp nhất đến mức cao nhất. 
3. Đối với từng vị trí bit$i$, tính toán xem hiện tại chúng ta có cần một đơn vị$2^i$từ$w$. 
4. Thêm tiền có sẵn`cnt[i]`đến một “nguồn cung” đang hoạt động ở mức đó. Nguồn cung này đại diện cho bao nhiêu đơn vị$2^i$chúng ta có thể sử dụng 
5. Nếu bit hiện tại của$w$là 1, chúng ta phải tiêu thụ một đơn vị$2^i$. Nếu nguồn cung bằng 0 thì câu trả lời ngay lập tức là KHÔNG vì chúng ta không thể thỏa mãn được bit này. 
6. Sau khi đáp ứng yêu cầu, mọi nguồn cung còn lại ở mức$i$được đưa lên trên như$2^{i+1}$, kể từ hai$2^i$có thể tạo thành một$2^{i+1}$. 
7. Tiếp tục quá trình này đến bit tối đa$w$, và cuối cùng kiểm tra xem phần mang còn sót lại là không liên quan hay có thể được hấp thụ. 

Ý tưởng chính là chúng ta không bao giờ loại bỏ những thứ dư thừa; mọi thứ đều lan truyền lên trên giống hệt như phép cộng nhị phân với việc gộp tài nguyên. 

### Tại sao nó hoạt động 

Tại mỗi vị trí bit$i$, mọi đóng góp cho giá trị đó đều không thể phân biệt được: chúng đều là$2^i$. Bất kỳ sự ghép nối hoặc phần còn lại nào sẽ tự động chuyển đổi thành bit tiếp theo bởi vì$2 \cdot 2^i = 2^{i+1}$. Điều này thực thi một hệ thống phân cấp chặt chẽ trong đó các quyết định ở các bit thấp hơn xác định đầy đủ tính khả thi mà không cần quay lại. Mức tiêu thụ tham lam ở mỗi bit là an toàn vì việc không đáp ứng được bit yêu cầu không thể được khắc phục bằng các bit cao hơn, trong khi phần dư thừa luôn có thể tái sử dụng một cách tối ưu thông qua mang. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    m = int(input())
    cnt = [0] * 31

    for _ in range(m):
        t, v = map(int, input().split())

        if t == 1:
            cnt[v] += 1
        else:
            w = v
            carry = 0
            ok = True

            for i in range(31):
                carry += cnt[i]

                if (w >> i) & 1:
                    if carry == 0:
                        ok = False
                        break
                    carry -= 1

                carry //= 2

            if ok:
                print("YES")
            else:
                print("NO")

if __name__ == "__main__":
    solve()
```Việc triển khai giữ một mảng tần số cố định có kích thước 31, vì$2^{30}$là đủ để bao gồm tất cả các giá trị lên đến$10^9$. Mỗi truy vấn GET mô phỏng cấu trúc nhị phân của mục tiêu bằng cách sử dụng một lệnh đang chạy. 

Chi tiết quan trọng là xử lý các bit theo thứ tự tăng dần. Số mang phải luôn được chia cho hai sau mỗi cấp độ để phản ánh sự tổng hợp thành lũy thừa tiếp theo của hai. Việc không áp dụng cách phân chia này một cách chính xác sẽ dẫn đến việc tính toán quá mức tài nguyên ở các cấp độ cao hơn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
1 0
1 0
1 0
2 3
2 4
```Sự phát triển của trạng thái: 

| Bước | Hoạt động | cnt[0] | mang theo | xử lý bit | kết quả | 
| --- | --- | --- | --- | --- | --- | 
| 1 | thêm 0 | 1 | - | - | - | 
| 2 | thêm 0 | 2 | - | - | - | 
| 3 | thêm 0 | 3 | - | - | - | 
| 4 | được 3 | 3 | 3→1→0 | bit 0,1 | CÓ | 
| 5 | được 4 | 3 | không đủ ở bit 2 | - | KHÔNG | 

GET đầu tiên thành công vì ba cái có thể tạo thành nhị phân 3. GET thứ hai thất bại vì không có cách nào để tạo ra một$2^2$đơn vị. 

### Ví dụ 2 

đầu vào:```
4
1 1
1 2
2 6
2 7
```| Bước | Hoạt động | cnt[1] | cnt[2] | mang theo | kết quả | 
| --- | --- | --- | --- | --- | --- | 
| 1 | thêm 1 | 1 | 0 | - | - | 
| 2 | thêm 2 | 1 | 1 | - | - | 
| 3 | được 6 | 1 | 1 | đủ | CÓ | 
| 4 | nhận được 7 | 1 | 1 | không đủ | KHÔNG | 

Điều này chứng tỏ việc kết hợp các quyền hạn khác nhau đòi hỏi phải có sự lan truyền mang chính xác như thế nào thay vì kiểm tra bit độc lập. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(30m)$| Mỗi truy vấn xử lý tối đa 31 vị trí bit | 
| Không gian |$O(30)$| Mảng cố định cho số mũ | 

Tổng công việc là khoảng$3 \times 10^6$hoạt động nằm trong giới hạn cho phép$10^5$truy vấn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    m = int(input())
    cnt = [0] * 31
    out = []

    for _ in range(m):
        t, v = map(int, input().split())
        if t == 1:
            cnt[v] += 1
        else:
            w = v
            carry = 0
            ok = True
            for i in range(31):
                carry += cnt[i]
                if (w >> i) & 1:
                    if carry == 0:
                        ok = False
                        break
                    carry -= 1
                carry //= 2
            out.append("YES" if ok else "NO")

    return "\n".join(out)

# provided sample
assert run("""5
1 0
1 0
1 0
2 3
2 4
""") == "YES\nNO"

# all equal small powers
assert run("""3
1 2
1 2
2 4
""") == "YES"

# impossible high bit
assert run("""2
1 0
2 2
""") == "NO"

# direct match
assert run("""2
1 5
2 32
""") == "YES"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| ba số 1 tạo thành 3 | CÓ | đội hình mang cơ bản | 
| hai số 4 tạo thành 8 | CÓ | mang bit cao hơn | 
| đơn 1 không làm được 2 | KHÔNG | không thể | 
| truy vấn 2^5 đơn | CÓ | trận đấu trực tiếp | 

## Vỏ cạnh 

Trường hợp một cạnh xảy ra khi nhiều năng lượng nhỏ tích tụ và phải xếp tầng qua nhiều lần mang. Ví dụ, chèn tám bản sao của$2^0$nên ngay lập tức cư xử như một người độc thân$2^3$. Thuật toán xử lý điều này vì mỗi cấp thực hiện phép chia số nguyên cho hai, truyền số dư lên trên nhiều lần cho đến khi ổn định. 

Một trường hợp khác là khi truy vấn GET yêu cầu một giá trị lớn hơn bất kỳ số mũ nào được lưu trữ. Ngay cả khi không có sức mạnh phù hợp duy nhất nào tồn tại, sự kết hợp vẫn có thể hoạt động. Ví dụ, hai$2^3$mục có thể đáp ứng$2^4$. Cơ chế mang đảm bảo rằng những sự hợp nhất này luôn được tính đến trước khi đánh giá các bit cao hơn, do đó thuật toán không bao giờ từ chối sai các cấu trúc hợp lệ.
