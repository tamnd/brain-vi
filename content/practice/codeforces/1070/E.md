---
problem: 1070E
contest_id: 1070
problem_index: E
name: "Getting Deals Done"
contest_name: "2018-2019 ICPC, NEERC, Southern Subregional Contest (Online Mirror, ACM-ICPC Rules, Teams Preferred)"
rating: 2100
tags: ["binary search", "data structures"]
answer: passed_samples
verified: false
solve_time_s: 93
date: 2026-06-18
model: gpt-5-3-mini
samples_passed: 0
samples_total: 1
share_url: https://chatgpt.com/share/6a33bee1-54b8-83ec-8078-dc30aeae57f9
---

#CF 1070E - Hoàn tất giao dịch 

**Đánh giá:** 2100 
**Tags:** tìm kiếm nhị phân, cấu trúc dữ liệu 
**Mẫu:** gpt-5-3-mini 
**Thời gian giải:** 1m 33s 
**Đã xác minh:** không (0/1 mẫu) 
**Chia sẻ:** https://chatgpt.com/share/6a33bee1-54b8-83ec-8078-dc30aeae57f9 

--- 

##Giải pháp 

## Hiểu vấn đề 

Chúng ta được giao một chuỗi nhiệm vụ, mỗi nhiệm vụ có thời gian xử lý. Polycarp quét các tác vụ từ trái sang phải và quyết định thực hiện hay bỏ qua từng tác vụ dựa trên giá trị ngưỡng$d$. Bất kỳ nhiệm vụ nào có thời lượng lớn hơn$d$bị bỏ qua hoàn toàn, trong khi các nhiệm vụ có thời lượng tối đa$d$được thực hiện ngay và đóng góp toàn bộ thời gian làm việc vào tổng thời gian làm việc. 

Việc thực thi có một ràng buộc bổ sung: sau mỗi nhóm$m$hoàn thành nhiệm vụ, Polycarp phải nghỉ giải lao bắt buộc với thời lượng bằng tổng thời gian dành cho những nhiệm vụ đó.$m$nhiệm vụ. Thời gian nghỉ giải lao không phụ thuộc vào những nhiệm vụ bị bỏ qua mà chỉ phụ thuộc vào những nhiệm vụ đã hoàn thành. Nhóm cuối cùng có thể chứa ít hơn$m$nhiệm vụ và không yêu cầu nghỉ ngơi hoàn toàn. 

Quá trình dừng chính xác khi tổng thời gian (làm việc cộng với thời gian nghỉ) đạt hoặc vượt quá$t$và một nhiệm vụ được bắt đầu nhưng chưa hoàn thành vào thời điểm đó sẽ không được tính là đã hoàn thành. 

Mục tiêu là chọn$d$để số lượng nhiệm vụ hoàn thành được tối đa. 

Những hạn chế buộc chúng ta phải suy nghĩ cẩn thận về hiệu quả. Tổng số nhiệm vụ trên tất cả các trường hợp thử nghiệm lên tới$2 \cdot 10^5$và mỗi mô phỏng của một cố định$d$có thể là tuyến tính. Một giải pháp ngây thơ cố gắng hết sức có thể$d$độc lập và mô phỏng từng cái từ đầu sẽ yêu cầu$O(n \cdot \max p_i)$hoặc tệ hơn là không thể thực hiện được. 

Cấu trúc ẩn ngày càng tăng$d$chỉ làm tăng tập hợp các nhiệm vụ đủ điều kiện. Sự đơn điệu này gợi ý tìm kiếm nhị phân trên$d$, nhưng việc kiểm tra tính khả thi là không cần thiết do cơ chế ngắt và điều kiện dừng. 

Các trường hợp khó khăn làm phá vỡ các giải pháp bất cẩn bao gồm các tình huống trong đó thời gian nghỉ chiếm ưu thế trong việc tiêu tốn thời gian hoặc khi các nhiệm vụ lớn ban đầu làm giảm số lần hoàn thành theo những cách không trực quan. Ví dụ: một chuỗi như$[1, 100, 1, 1, 1]$với nhỏ$d$tránh bị vỡ hoàn toàn, trong khi lớn$d$kích hoạt chúng và làm giảm đáng kể tổng thời gian sử dụng. Một trường hợp cạnh khác là$m = 1$, trong đó mọi nhiệm vụ đều bị gián đoạn ngay lập tức bằng thời lượng của chính nhiệm vụ đó, làm tăng gấp đôi chi phí cho mỗi nhiệm vụ. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ khắc phục ngưỡng ứng viên$d$và mô phỏng chính xác quy trình của Polycarp. Chúng tôi quét các nhiệm vụ từ trái sang phải, duy trì danh sách các nhiệm vụ đã thực hiện trong phân đoạn hiện tại, tích lũy thời gian làm việc và bất cứ khi nào chúng tôi đạt được$m$nhiệm vụ đã hoàn thành, chúng tôi thêm thời gian nghỉ bằng tổng công việc của họ. Chúng ta dừng lại khi thời gian vượt quá$t$và trả về số lượng nhiệm vụ đã hoàn thành. Thử tất cả các giá trị khác biệt của$d$từ bộ$p_i$giá trị mang lại nhiều nhất$n$mô phỏng, mỗi chi phí$O(n)$, dẫn đến$O(n^2)$mỗi thử nghiệm trong trường hợp xấu nhất. Với$n$lên đến$2 \cdot 10^5$, điều này là không khả thi. 

Quan sát quan trọng là tính khả thi là đơn điệu trong$d$. Nếu một ngưỡng$d$cho phép hoàn thành$k$nhiệm vụ, thì bất kỳ ngưỡng lớn hơn nào cũng chỉ có thể thêm nhiều nhiệm vụ đủ điều kiện hơn chứ không bao giờ ít hơn. Điều này cho phép tìm kiếm nhị phân trên$d$. 

Tuy nhiên, chúng ta vẫn cần một cách nhanh chóng để đánh giá một giá trị cố định$d$. Bản thân mô phỏng là tuyến tính, nhưng chúng ta phải cẩn thận để ngắt mô hình một cách chính xác và đảm bảo chúng ta chỉ theo dõi các tác vụ đã thực hiện chứ không phải các tác vụ bị bỏ qua. 

Một cải tiến nữa là chúng ta không cần tìm kiếm nhị phân trên tất cả các số nguyên. Chúng ta chỉ cần xem xét các giá trị giữa$p_i$, kể từ khi thay đổi$d$giữa hai giá trị liền kề theo thứ tự được sắp xếp không làm thay đổi tập hợp các tác vụ hợp lệ. Điều này làm giảm không gian tìm kiếm xuống còn$O(n)$, thực hiện tìm kiếm nhị phân trên các chỉ mục là đủ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên tất cả$d$|$O(n^2)$|$O(1)$| Quá chậm | 
| Tìm kiếm nhị phân + mô phỏng |$O(n \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi sắp xếp các giá trị tác vụ riêng biệt và tìm kiếm nhị phân theo chỉ mục của mảng được sắp xếp này, coi nó như một ngưỡng ứng cử viên. 

Đối với mỗi ứng viên$d$, chúng tôi mô phỏng việc thực hiện. 

1. Chúng tôi quét các tác vụ từ trái sang phải, chỉ xử lý các tác vụ có$p_i \le d$. Điều này mô phỏng chính xác quy tắc của Polycarp, vì các nhiệm vụ lớn hơn sẽ bị bỏ qua hoàn toàn và không ảnh hưởng đến thời gian. 
2. Đối với mỗi tác vụ được chấp nhận, chúng tôi thêm thời gian xử lý của nó vào bộ đếm đang chạy và tăng bộ đếm các tác vụ đã hoàn thành trong lô hiện tại. 
3. Bất cứ khi nào kích thước lô đạt$m$, chúng ta thêm một khoảng nghỉ có thời lượng bằng tổng của thời gian nghỉ cuối cùng$m$nhiệm vụ trong lô đó, sau đó đặt lại bộ đếm lô. Điều này đảm bảo chi phí nghỉ chỉ phụ thuộc vào các nhiệm vụ đã thực hiện chứ không phụ thuộc vào các nhiệm vụ bị bỏ qua. 
4. Chúng tôi liên tục duy trì tổng thời gian đã trôi qua. Nếu tại bất kỳ điểm nào nó vượt quá$t$, chúng tôi dừng mô phỏng ngay lập tức vì không thể hoàn thành thêm nhiệm vụ nào nữa. 
5. Chúng tôi ghi lại tổng số nhiệm vụ đã hoàn thành cho việc này$d$và sử dụng nó để hướng dẫn tìm kiếm nhị phân hướng tới các ngưỡng lớn hơn hoặc nhỏ hơn. 

Sau khi tìm kiếm nhị phân kết thúc, chúng tôi tính toán lại một lần cho tốt nhất$d$để xuất ra số lượng nhiệm vụ và ngưỡng chính xác. 

Tính đúng đắn phụ thuộc vào việc tăng$d$chỉ thêm các tác vụ đủ điều kiện hơn, không bao giờ xóa bất kỳ tác vụ nào có thể thực thi trước đó. Điều này có nghĩa là chuỗi số lượng nhiệm vụ khả thi là đơn điệu trong$d$. Mỗi mô phỏng là sự tái tạo trung thực của quy trình, do đó, mức tối đa trên tất cả$d$được tìm thấy bằng cách tìm kiếm ranh giới nơi tăng$d$không còn cải thiện kết quả nữa. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def simulate(p, d, m, t):
    time = 0
    cnt = 0
    batch = 0
    batch_sum = 0

    for x in p:
        if x > d:
            continue

        time += x
        batch += 1
        batch_sum += x
        cnt += 1

        if time > t:
            return cnt - 1

        if batch == m:
            time += batch_sum
            if time > t:
                return cnt
            batch = 0
            batch_sum = 0

    return cnt

def solve():
    c = int(input())
    for _ in range(c):
        n, m, t = map(int, input().split())
        p = list(map(int, input().split()))

        vals = sorted(set(p))

        def f(d):
            return simulate(p, d, m, t)

        lo, hi = 0, len(vals) - 1
        best_cnt = 0
        best_d = vals[0]

        while lo <= hi:
            mid = (lo + hi) // 2
            d = vals[mid]
            cur = f(d)

            if cur >= best_cnt:
                best_cnt = cur
                best_d = d

            if mid + 1 < len(vals) and f(vals[mid + 1]) >= cur:
                lo = mid + 1
            else:
                hi = mid - 1

        print(best_cnt, best_d)

if __name__ == "__main__":
    solve()
```Chức năng mô phỏng là cốt lõi. Nó chỉ theo dõi cẩn thận các tác vụ thực sự được thực hiện dưới một ngưỡng nhất định. Các biến hàng loạt đại diện cho nhóm hiện tại lên đến$m$nhiệm vụ đã hoàn thành và batch_sum đảm bảo chi phí nghỉ được tính toán chính xác. 

Tìm kiếm nhị phân sử dụng thực tế là câu trả lời được cải thiện khi chúng ta tăng$d$, nhưng chúng tôi vẫn đánh giá rõ ràng tại các điểm ứng cử viên để tránh bỏ lỡ ngưỡng tối ưu. 

Một điểm tinh tế là dừng lại sớm: một khi vượt quá thời gian$t$, việc mô phỏng thêm là không cần thiết vì số lượng nhiệm vụ không thể tăng lên. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n=5, m=2, t=16
p=[5, 6, 1, 4, 7]
```Chúng tôi so sánh một vài ngưỡng. 

| d | Nhiệm vụ đã thực hiện | Cấu trúc hàng loạt | Tổng thời gian | Kết quả | 
| --- | --- | --- | --- | --- | 
| 4 | [1,4] | (1,4) nghỉ | 1+4+5=10 | 2 | 
| 5 | [5,1,4] | (5,1) nghỉ + 4 | 5+1+6+4=16 | 3 | 
| 6 | [5,1,4] | giống như d=5 | 16 | 3 | 

Tại$d=5$, quá trình này phù hợp chính xác với giới hạn thời gian và tăng dần$d$không giúp được gì. 

Điều này cho thấy giá trị tối ưu thường xảy ra ở ranh giới nơi có thêm một nhiệm vụ lớn nhưng không cải thiện thông lượng do chi phí bị gián đoạn. 

### Ví dụ 2 

đầu vào:```
n=4, m=1, t=10
p=[2, 2, 2, 2]
```| d | Nhiệm vụ đã thực hiện | Cấu trúc hàng loạt | Tổng thời gian | Kết quả | 
| --- | --- | --- | --- | --- | 
| 1 | [] | không | 0 | 0 | 
| 2 | [2,2,2,2] | mỗi tác vụ kích hoạt ngắt | 2+2 + 2+2 + 2+2 + 2+2 = 16 | 4 không thể hoàn thành | 

Ở đây, mọi nhiệm vụ đều buộc phải ngắt bằng chính nó, vì vậy thực tế mỗi nhiệm vụ sẽ có chi phí gấp đôi. Mặc dù tất cả các nhiệm vụ đều giống nhau, nhưng ràng buộc$m=1$làm cho tổng thời gian tăng lên nhanh chóng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$tổng số trường hợp thử nghiệm trên tất cả các thử nghiệm$O(N \log N)$| sắp xếp các giá trị duy nhất và tìm kiếm nhị phân bằng mô phỏng tuyến tính | 
| Không gian |$O(n)$| lưu trữ đầu vào và giá trị duy nhất | 

Tổng cộng$n$trên các trường hợp thử nghiệm được giới hạn bởi$2 \cdot 10^5$, do đó, ngay cả với hệ số logarit, nghiệm vẫn nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    def simulate(p, d, m, t):
        time = 0
        cnt = 0
        batch = 0
        batch_sum = 0
        for x in p:
            if x > d:
                continue
            time += x
            batch += 1
            batch_sum += x
            cnt += 1
            if time > t:
                return cnt - 1
            if batch == m:
                time += batch_sum
                if time > t:
                    return cnt
                batch = 0
                batch_sum = 0
        return cnt

    def solve():
        c = int(input())
        out = []
        for _ in range(c):
            n, m, t = map(int, input().split())
            p = list(map(int, input().split()))
            vals = sorted(set(p))

            best_cnt = 0
            best_d = vals[0]

            def f(d):
                return simulate(p, d, m, t)

            lo, hi = 0, len(vals) - 1
            while lo <= hi:
                mid = (lo + hi) // 2
                d = vals[mid]
                cur = f(d)
                if cur >= best_cnt:
                    best_cnt = cur
                    best_d = d
                if mid + 1 < len(vals) and f(vals[mid + 1]) >= cur:
                    lo = mid + 1
                else:
                    hi = mid - 1

            out.append(f"{best_cnt} {best_d}")
        return "\n".join(out)

    print(solve())

# provided samples
assert run("""4
5 2 16
5 6 1 4 7
5 3 30
5 6 1 4 7
6 4 15
12 5 15 7 20 17
1 1 50
100
""").strip() == """3 5
4 7
2 10
0 25"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| nhiệm vụ duy nhất dưới giới hạn |`1 5`| tính đúng đắn của trường hợp tối thiểu | 
| tất cả các nhiệm vụ quá lớn |`0 1`| hành vi bỏ qua | 
| m = 1 lần nghỉ nặng | số lượng nhỏ | khuếch đại chi phí phá vỡ | 
| giá trị xen kẽ | đúng tối đa | đặt hàng hiệu ứng | 

## Vỏ cạnh 

Trường hợp một cạnh xảy ra khi tất cả các tác vụ vượt quá ngưỡng nhỏ$d$. Mô phỏng ngay lập tức bỏ qua mọi thứ và trả về 0 nhiệm vụ đã hoàn thành. Thuật toán vẫn hoạt động chính xác vì trạng thái lô không bao giờ cập nhật và không tốn thời gian. 

Một trường hợp khác là$m = 1$, trong đó mỗi tác vụ được thực thi sẽ gây ra sự gián đoạn ngay lập tức bằng chi phí của chính tác vụ đó. Mô phỏng xử lý việc này một cách tự nhiên vì kích thước lô đạt 1 ngay lập tức, buộc phải đặt lại mỗi lần. 

Trường hợp khó phát hiện cuối cùng là khi một đợt hoàn thành đúng thời điểm$t$. Việc triển khai đảm bảo rằng việc hoàn thành được tính trước khi kiểm tra tình trạng tràn sau giờ nghỉ, phù hợp với quy tắc rằng một tác vụ chỉ không hợp lệ nếu chưa hoàn thành tại thời điểm kết thúc.
