---
title: "CF 105672B - Gấu Cú Ngốc"
description: "Chúng ta được cấp một bộ đại bác, mỗi khẩu đại bác tạo ra một lượng sát thương cố định mỗi lần bắn. Thời gian diễn ra theo từng giây riêng biệt và ở mỗi giây tất cả các khẩu pháo đều bắn đồng thời."
date: "2026-06-26T09:54:45+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105672
codeforces_index: "B"
codeforces_contest_name: "TheForces Round #39 (1000-Forces)"
rating: 0
weight: 105672
solve_time_s: 48
verified: true
draft: false
---

[CF 105672B - Gấu Cú Ngốc](https://codeforces.com/problemset/problem/105672/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 48s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một bộ đại bác, mỗi khẩu đại bác tạo ra một lượng sát thương cố định mỗi lần bắn. Thời gian diễn ra theo từng giây riêng biệt và ở mỗi giây tất cả các khẩu pháo đều bắn đồng thời. Tuy nhiên, OwlBear có một tấm khiên xoay: ở giây thứ k, nó chặn chính xác một khẩu pháo, cụ thể là khẩu pháo được chỉ số bởi (k−1) mod n, do đó khẩu pháo đó không gây sát thương ở giây đó trong khi tất cả những khẩu khác vẫn gây toàn bộ sát thương. 

Với mỗi giá trị truy vấn h, chúng ta cần xác định số giây t nhỏ nhất sao cho tổng thiệt hại tích lũy gây ra trong t giây đầu tiên ít nhất là h. 

Điểm tinh tế quan trọng là khẩu pháo bị chặn thay đổi theo chu kỳ mỗi giây, do đó, mỗi khẩu pháo chỉ bị chặn một lần trong n giây và trong thời gian dài, mỗi khẩu pháo đều bị chặn đều. Điều này tạo ra một khuôn mẫu lặp lại trong các đóng góp. 

Các ràng buộc ngụ ý n lên tới 100000 và tổng n cộng q qua các thử nghiệm lên tới 200000, do đó, mọi giải pháp đều phải gần tuyến tính hoặc n log n cho mỗi trường hợp thử nghiệm. Không thể mô phỏng đơn giản tất cả các giây vì h có thể lên tới 10^9 và sát thương mỗi giây có thể đủ lớn để các câu trả lời có thể lên tới t rất lớn. 

Một cách tiếp cận ngây thơ có thể cố gắng mô phỏng từng giây một, tính toán lại tổng của tất cả các khẩu pháo đang hoạt động mỗi lần và tích lũy cho đến khi đạt h. Điều này không thành công vì mỗi giây tốn O(n) và trong trường hợp xấu nhất t cũng có thể lớn, dẫn đến O(n·t), điều này hoàn toàn không khả thi. 

Một lực lượng vũ phu cẩn thận hơn sẽ cải thiện điều này một chút bằng cách tính toán trước tổng tổng của tất cả các khẩu pháo và trừ đi số khẩu bị chặn mỗi giây, nhưng việc vẫn mô phỏng từng giây khiến nó quá chậm đối với các câu trả lời lớn. 

Trường hợp cạnh ít rõ ràng hơn xuất hiện khi h nhỏ. Ví dụ: nếu h nhỏ hơn mức thiệt hại tối đa có thể xảy ra trong một giây, thì câu trả lời là 1. Bất kỳ giải pháp nào giả định ít nhất một chu kỳ đầy đủ là bắt buộc sẽ thất bại ở đây. 

Một trường hợp cạnh khác xuất hiện khi n = 2. Tấm chắn luân phiên chặn giữa hai khẩu pháo nên mỗi giây sẽ loại bỏ một phần tử khác nhau. Mô hình này trở nên rất đều đặn và những sai sót trong việc xử lý việc lập chỉ mục modulo thường dẫn đến các lỗi sai lệch trong đó khẩu pháo bị chặn ở giây nào. 

## Phương pháp tiếp cận 

Quan điểm vũ phu rất đơn giản. Tại mỗi giây k, chúng tôi tính tổng thiệt hại là tổng của tất cả a[i] ngoại trừ a[(k−1) mod n], sau đó giữ tổng tiền tố đang chạy cho đến khi đạt đến h. Điều này đúng vì nó tuân theo chính xác các quy tắc, nhưng tính tổng từ đầu mỗi giây tốn O(n), làm cho độ phức tạp tổng cộng là O(n·t). Vì t có thể lớn và h không bị giới hạn tới 10^9 nên cách tiếp cận này sẽ hết thời gian ngay lập tức. 

Quan sát quan trọng là tổng tổng của tất cả các khẩu pháo là không đổi, gọi nó là S. Tại mỗi giây, chính xác một khẩu pháo bị loại trừ, do đó sát thương ở giây k là S − a[(k−1) mod n]. Trong toàn bộ chu kỳ n giây, mỗi khẩu pháo bị loại trừ đúng một lần, do đó tổng sát thương của toàn bộ chu kỳ là n·S − S = (n−1)S. 

Điều này làm giảm cấu trúc bài toán từ “tính toán lại động” thành một mảng lặp cố định có độ dài n, trong đó mỗi phần tử là S − a[i]. Sau đó, chúng ta cần tìm độ dài tiền tố nhỏ nhất có tổng đạt đến h, trở thành bài toán tổng tiền tố tiêu chuẩn + tìm kiếm nhị phân trên một mảng tuần hoàn vô hạn. Chúng tôi chỉ cần xử lý các chu trình đầy đủ và tiền tố chu trình một phần, cho phép chúng tôi tính toán các câu trả lời trong O(log n) cho mỗi truy vấn sau khi xử lý trước. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(n·t) | O(1) | Quá chậm | 
| Tiền tố + Tìm kiếm nhị phân theo mẫu tuần hoàn | O(n + q log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Tính tổng S của tất cả các thiệt hại do pháo gây ra. Điều này thể hiện mức độ sát thương trong một giây nếu không có khẩu pháo nào bị chặn. 
2. Xây dựng một mảng b trong đó b[i] = S − a[i]. Điều này thể hiện mức độ thiệt hại thực tế ở thời điểm pháo thứ i bị chặn. 
3. Xây dựng mảng tổng tiền tố trên b cho một chu kỳ đầy đủ. Điều này cho phép tính toán nhanh thiệt hại trên bất kỳ tiền tố nào của chu kỳ. 
4. Tính tổng thiệt hại trong một chu kỳ đầy đủ như Cycle_sum = sum(b). 
5. Đối với mỗi truy vấn h, trước tiên hãy xác định cần bao nhiêu chu kỳ đầy đủ bằng cách chia h cho Cycle_sum. Mỗi chu kỳ đầy đủ đóng góp một lượng sát thương cố định, vì vậy chúng ta có thể nhảy trực tiếp qua nhiều giây mà không cần mô phỏng. 
6. Giảm sát thương cần thiết còn lại xuống giá trị còn lại sau toàn bộ chu kỳ. 
7. Trên chu kỳ một phần còn lại, tìm kiếm nhị phân độ dài tiền tố nhỏ nhất trong b được lặp lại theo chu kỳ có tổng tiền tố đạt đến mức thiệt hại còn lại. Điều này có tác dụng vì trong một chu kỳ, trình tự cố định và tổng tiền tố là đơn điệu. 

### Tại sao nó hoạt động 

Điều bất biến cốt lõi là sát thương của mỗi giây chỉ phụ thuộc vào chỉ mục nào hiện đang bị chặn và kiểu chặn đó lặp lại sau mỗi n giây. Điều này đảm bảo rằng chuỗi thiệt hại tính theo giây là tuần hoàn với chu kỳ n. Khi điều này được nhận ra, toàn bộ quá trình sẽ tương đương với việc tìm tổng tiền tố qua sự lặp lại vô hạn của một mảng cố định. Vì tổng tiền tố trên các mảng định kỳ phân rã thành các chu kỳ đầy đủ cộng với một phần chu trình, nên chúng ta luôn có thể tách câu trả lời thành một số tuyến tính của các chu kỳ đầy đủ cộng với tìm kiếm giới hạn trong một chu kỳ. Cấu trúc này đảm bảo rằng không có bước tìm kiếm tham lam hoặc nhị phân nào bỏ qua tiền tố ngắn hơn hợp lệ, bởi vì tổng tiền tố trong mảng được xây dựng này là đơn điệu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    out = []

    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        q = int(input())
        queries = [int(input()) for _ in range(q)]

        S = sum(a)
        b = [S - x for x in a]

        pref = [0] * (n + 1)
        for i in range(n):
            pref[i + 1] = pref[i] + b[i]

        cycle_sum = pref[n]

        for h in queries:
            full = h // cycle_sum
            rem = h % cycle_sum

            if rem == 0:
                out.append(str(full * n))
                continue

            lo, hi = 1, n
            ans = n
            while lo <= hi:
                mid = (lo + hi) // 2
                if pref[mid] >= rem:
                    ans = mid
                    hi = mid - 1
                else:
                    lo = mid + 1

            out.append(str(full * n + ans))

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách nén thiệt hại trong mỗi giây thành một giá trị duy nhất cho mỗi chỉ mục bị chặn. Mảng tiền tố trên b ghi lại thiệt hại tích lũy trong một chu kỳ và Cycle_sum ghi lại sự đóng góp của toàn bộ các vòng quay. Đối với mỗi truy vấn, số lượng chu kỳ đầy đủ được trích xuất bằng phép chia, điều này tránh mọi mô phỏng. 

Bước tìm kiếm nhị phân rất quan trọng vì trong một chu kỳ, sự tích lũy thiệt hại sẽ tăng dần theo thời gian. Điều này đảm bảo rằng khi tổng tiền tố vượt quá yêu cầu còn lại thì tất cả các tiền tố dài hơn cũng đáp ứng yêu cầu đó, khiến cho việc tìm kiếm trở nên hợp lệ. 

Một lỗi phổ biến ở đây là quên rằng chỉ mục bị chặn sẽ đặt lại sau mỗi n giây, không tăng trên toàn cầu nếu không có sự bao bọc. Điều đó sẽ phá vỡ cấu trúc tuần hoàn một cách không chính xác và dẫn đến câu trả lời sai. 

## Ví dụ đã hoạt động 

Chúng tôi sử dụng đầu vào mẫu: 

n = 4, a = [2, 3, 2, 7] 

Đầu tiên hãy tính S = 14, do đó b = [12, 11, 12, 7] và Cycle_sum = 42. 

### Truy vấn 1: h = 27 

Ta tính full = 27 // 42 = 0, rem = 27. 

Chúng tôi tìm kiếm tiền tố của b: 

| giây | chỉ mục bị chặn | tổng tiền tố | 
| --- | --- | --- | 
| 1 | 0 | 12 | 
| 2 | 1 | 23 | 
| 3 | 2 | 35 | 

Ở giây thứ 3, ta đạt 35 ≥ 27 nên đáp án là 3. 

Điều này cho thấy một trường hợp chu kỳ một phần thuần túy. 

### Truy vấn 2: h = 12 

đầy đủ = 0, rem = 12. 

Ở giây đầu tiên, tiền tố đã là 12 nên câu trả lời là 1. 

Điều này xác nhận tính đúng đắn của các ngưỡng tối thiểu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + q log n) cho mỗi trường hợp thử nghiệm | xây dựng tổng tiền tố là tuyến tính, mỗi truy vấn sử dụng tìm kiếm nhị phân | 
| Không gian | O(n) | lưu trữ mảng b và tổng tiền tố | 

Các ràng buộc cho phép tổng cộng 2e5 trên tất cả n và q, do đó quá trình tiền xử lý tuyến tính với các truy vấn logarit này vừa vặn thoải mái trong các giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdin.read()

# Since full solution is inside solve(), in real setup you'd import and call it.
# These are structural test placeholders.

# sample (conceptual)
# assert run(...) == ...

# custom cases
# n=2 alternating
# all equal values
# single query small h
# large h requiring multiple cycles
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tối thiểu n=2 xen kẽ | xử lý tiền tố đúng | tính chính xác của việc chặn modulo | 
| tất cả đều bằng a[i] | hành vi chu kỳ thống nhất | trường hợp đối xứng | 
| h rất nhỏ | 1 | chấm dứt ngay lập tức | 
| h lớn nhiều chu kỳ | bước nhảy lớn đúng đắn | phân hủy chu trình | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi thiệt hại cần thiết h nhỏ hơn mức đóng góp trong giây đầu tiên. Trong trường hợp đó, câu trả lời phải là 1 vì giây đầu tiên đã tạo đủ sát thương. Thuật toán xử lý việc này vì rem ≤ pref[1] ngay lập tức kích hoạt tìm kiếm nhị phân trả về 1. 

Một trường hợp khác là khi h là bội số chính xác của Cycle_sum. Trong trường hợp đó rem trở thành 0 và câu trả lời phải chính xác là các chu kỳ đầy đủ lần n giây. Mã xử lý điều này một cách rõ ràng bằng cách trả về đầy đủ * n, tránh tìm kiếm nhị phân không chính xác vào phần dư trống. 

Khi n = 2, chỉ số bị chặn thay đổi hoàn toàn trong khoảng từ 0 đến 1. Cấu trúc tiền tố vẫn hoạt động vì b mã hóa chính xác phần đóng góp của từng giây một cách độc lập, đảm bảo không vi phạm giả định nào về tính độc lập.
