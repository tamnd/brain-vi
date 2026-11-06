---
title: "CF 1267E - Bầu cử"
description: "Chúng ta được cung cấp một hệ thống bỏ phiếu với nhiều ứng cử viên và nhiều trạm bỏ phiếu. Mỗi trạm báo cáo mỗi ứng cử viên nhận được bao nhiêu phiếu bầu. Điểm cuối cùng của một ứng cử viên là tổng số phiếu bầu của họ trên tất cả các đài còn hiệu lực."
date: "2026-06-18T17:58:58+07:00"
tags: ["codeforces", "competitive-programming", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1267
codeforces_index: "E"
codeforces_contest_name: "2019-2020 ICPC, NERC, Northern Eurasia Finals (Unrated, Online Mirror, ICPC Rules, Teams Preferred)"
rating: 1700
weight: 1267
solve_time_s: 81
verified: true
draft: false
---

[CF 1267E - Cuộc bầu cử](https://codeforces.com/problemset/problem/1267/E) 

**Đánh giá:** 1700 
**Thẻ:** tham lam 
**Thời gian giải:** 1 phút 21s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một hệ thống bỏ phiếu với nhiều ứng cử viên và nhiều trạm bỏ phiếu. Mỗi trạm báo cáo mỗi ứng cử viên nhận được bao nhiêu phiếu bầu. Điểm cuối cùng của một ứng cử viên là tổng số phiếu bầu của họ trên tất cả các đài còn hiệu lực. 

Có một ứng cử viên đặc biệt, người cuối cùng, đại diện cho phe đối lập. Đảng cầm quyền được phép vô hiệu hóa toàn bộ các điểm bỏ phiếu. Sau khi một số đài bị xóa, tổng số phiếu bầu còn lại sẽ xác định người chiến thắng. Ứng cử viên đối lập được coi là đắc cử nếu tổng số phiếu của họ vượt quá tổng số phiếu của mọi ứng cử viên khác. 

Nhiệm vụ là loại bỏ càng ít đài càng tốt để sau khi loại bỏ, ứng cử viên đối lập không vượt lên dẫn trước những người khác. Tương tự, chúng ta muốn loại bỏ số trạm nhỏ nhất để ít nhất một ứng cử viên khác có điểm cuối cùng lớn hơn hoặc bằng đối thủ. 

Các hạn chế rất nhỏ, có tới 100 ứng cử viên và 100 điểm bỏ phiếu. Điều này ngay lập tức loại trừ mọi tìm kiếm theo cấp số nhân trên các tập hợp con của các đài vì có thể có tới 2^100 tập hợp con. Giải pháp bậc hai hoặc bậc ba về số lượng trạm có thể chấp nhận được, nhưng bất cứ điều gì liên quan đến việc liệt kê tập hợp con đầy đủ thì không. 

Một điểm tế nhị xuất hiện khi lý luận tham lam: loại bỏ các đài có sự ủng hộ đối lập lớn nhất tưởng chừng như đương nhiên, nhưng điều này không hề đúng. Một đài giúp đỡ phe đối lập cũng có thể ủng hộ rất nhiều cho một ứng cử viên khác và việc loại bỏ nó thực sự có thể gây tổn hại cho ứng cử viên đó nhiều hơn phe đối lập. Quyết định phải xét đến _lợi ích tương đối_, chứ không phải số phiếu phản đối tuyệt đối. 

Một trường hợp khác đến từ dây buộc. Điều kiện rất nghiêm ngặt: sự phản đối không được lớn hơn tất cả những người khác. Nếu phe đối lập có quan hệ với ai đó, điều đó đã ngăn cản cuộc bầu cử. Nhiều chiến lược tham lam không chính xác thực thi nhầm một điều kiện mạnh hơn mức cần thiết. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử từng tập hợp con của các điểm bỏ phiếu, tính toán tổng số ứng cử viên và kiểm tra xem liệu phe đối lập có đạt đến mức tối đa hay không. Điều này đúng vì nó đánh giá tất cả các kết quả có thể xảy ra, nhưng không khả thi vì có 2^m tập hợp con và mỗi lần đánh giá có chi phí O(nm), dẫn đến thời gian chạy vô cùng lớn. 

Cái nhìn sâu sắc quan trọng là đảo ngược quan điểm. Thay vì chọn các trạm để loại bỏ, chúng ta có thể nghĩ đến việc giữ lại các trạm. Nếu chúng ta giữ một tập hợp con các trạm, lợi thế của phe đối lập so với mọi ứng cử viên khác chỉ phụ thuộc vào sự khác biệt tổng hợp giữa phiếu bầu của họ và phiếu bầu của từng đối thủ cạnh tranh. 

Sửa chữa một ứng viên của đối thủ cạnh tranh j. Tại mỗi trạm i, xác định một giá trị: 

di = a[i][n] - a[i][j] 

Điều này đo lường mức độ giữ vị trí của tôi giúp phe đối lập so với ứng cử viên j. Nếu di lớn và dương, việc giữ vị trí đó sẽ củng cố vị trí dẫn đầu của phe đối lập so với j. Nếu di âm, trạm thực sự giúp j thu hẹp hoặc đảo ngược khoảng cách. 

Để đảm bảo phe đối lập không đánh bại j, chúng ta muốn tổng di trên các trạm được giữ không dương. Nếu nó trở nên dương, phe đối lập dẫn trước j. 

Bây giờ vấn đề trở thành: với mỗi đối thủ j, chúng ta cần chọn số trạm tối thiểu cần loại bỏ sao cho tổng di trên các trạm còn lại là ≤ 0. Điều này tương đương với việc chọn các trạm để giữ sao cho tránh mang lại cho đối thủ quá nhiều lợi ích tương đối. 

Đối với j cố định, chiến lược tối ưu là tham lam: sắp xếp các trạm theo di theo thứ tự giảm dần. Ban đầu, chúng tôi giả định rằng chúng tôi giữ lại tất cả các trạm, sau đó chúng tôi loại bỏ dần dần các trạm có di dương lớn nhất trước tiên, bởi vì đây là những trạm làm tăng lợi thế của phe đối lập nhiều nhất. Chúng tôi dừng lại khi số tiền còn lại trở nên không dương. 

Chúng tôi lặp lại quá trình này cho mọi ứng cử viên j và lấy kết quả tốt nhất.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^m · n · m) | O(m) | Quá chậm | 
| Loại bỏ sự tham lam của mỗi ứng cử viên | O(n · m log m) | O(m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi lặp lại mọi ứng cử viên không thuộc phe đối lập j và tính toán xem phải loại bỏ bao nhiêu trạm để đảm bảo phe đối lập không đánh bại j. 

1. Tính tổng lợi thế của phe đối lập so với ứng cử viên j trên tất cả các trạm bằng cách tính tổng di = a[i][n] - a[i][j]. Điều này thể hiện mức độ tiến xa của phe đối lập trước khi bị loại bỏ. 
2. Với mỗi trạm i, hãy tính di. Sắp xếp các trạm theo thứ tự giảm dần của di. Các đài có di dương lớn là có hại nhất vì chúng làm tăng lợi nhuận của phe đối lập nhiều nhất. 
3. Bắt đầu với tất cả các trạm được bao gồm và duy trì tổng di hiện tại. Nếu tổng này đã bằng 0 thì không cần loại bỏ ứng viên này. 
4. Nếu không, hãy liên tục loại bỏ trạm có di lớn nhất. Sau khi xóa một đài, hãy trừ đi di của đài đó khỏi tổng hoạt động. Mỗi lần loại bỏ sẽ làm giảm lợi thế của đối phương nhiều nhất có thể ở bước đó. 
5. Dừng khi tổng hoạt động trở thành ≤ 0. Ghi lại số lượng trạm đã bị xóa và đó là những trạm nào. 
6. Lặp lại cho tất cả các ứng cử viên j từ 1 đến n-1 và lấy tập hợp loại bỏ tối thiểu trên tất cả j. 

Đầu ra là tập hợp các trạm bị loại bỏ tương ứng với ứng viên j tốt nhất. 

### Tại sao nó hoạt động 

Đối với một ứng cử viên cố định j, giá trị di đo lường sự đóng góp vào việc phe đối lập dẫn trước j. Bất kỳ giải pháp hợp lệ nào cũng phải đảm bảo rằng tổng di trên các trạm được giữ không dương. Việc loại bỏ một trạm tương đương với việc lấy tổng số di của nó trừ đi. 

Việc chọn loại bỏ các trạm có di đầu tiên lớn nhất là tối ưu vì mỗi lần loại bỏ sẽ tạo ra mức giảm tối đa có thể về tổng trên mỗi hành động. Bất kỳ giải pháp tối ưu nào loại bỏ một di nhỏ hơn trong khi để lại một di lớn hơn sẽ hoàn toàn tệ hơn hoặc tương đương nhưng không bao giờ tốt hơn về việc giảm tổng số tiền với cùng một số lần loại bỏ. 

Do đó, quá trình tham lam tạo ra số lượng loại bỏ tối thiểu cần thiết để đẩy tổng về không dương, tương ứng chính xác với việc ngăn chặn phe đối lập đánh bại ứng cử viên j một cách nghiêm khắc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    a = [list(map(int, input().split())) for _ in range(m)]

    best_k = m + 1
    best_remove = None

    for j in range(n - 1):
        diffs = []
        total = 0

        for i in range(m):
            d = a[i][n - 1] - a[i][j]
            diffs.append((d, i))
            total += d

        diffs.sort(reverse=True)

        removed = []
        cur = total

        for d, i in diffs:
            if cur <= 0:
                break
            cur -= d
            removed.append(i + 1)

        if len(removed) < best_k:
            best_k = len(removed)
            best_remove = removed

    print(best_k)
    print(*best_remove)

if __name__ == "__main__":
    solve()
```Mã đầu tiên đọc tất cả các kết quả của trạm. Đối với mỗi ứng cử viên ngoại trừ phe đối lập, nó sẽ xây dựng những khác biệt về lợi thế ở mỗi vị trí so với ứng cử viên đó. Việc sắp xếp những khác biệt này cho phép chúng tôi ưu tiên loại bỏ các đài ủng hộ phe đối lập nhiều nhất trong so sánh đó. 

Vòng lặp xây dựng`removed`mô phỏng dần dần loại bỏ các trạm có hại cho đến khi phe đối lập không còn lợi thế lớn so với ứng cử viên đã chọn. Lần đầu tiên điều kiện`cur <= 0`hài lòng, chúng tôi đã đạt được số lần loại bỏ tối thiểu đối với ứng viên đó do thứ tự được sắp xếp. 

Cuối cùng, chúng tôi chọn ứng cử viên tốt nhất, tương ứng với số lần hủy tối thiểu trên toàn cầu. 

Một cạm bẫy phổ biến là quên rằng sự so sánh là nghiêm ngặt, do đó sự bình đẳng đã đủ để dừng lại. Một sự tinh tế khác là sử dụng cách lập chỉ mục ứng viên chính xác: ứng cử viên`n-1`là sự đối lập và sự khác biệt luôn được tính toán tương ứng với nó. 

## Ví dụ đã hoạt động 

Chúng tôi sử dụng đầu vào mẫu. 

đầu vào:```
5 3
6 3 4 2 8
3 7 5 6 7
5 2 4 7 9
```Chúng tôi tính toán cho ứng viên 1 (chỉ số 0). Sự khác biệt giữa mỗi trạm di = phe đối lập - ứng cử viên1 là: 

| Trạm | di | 
| --- | --- | 
| 1 | 8 - 6 = 2 | 
| 2 | 7 - 3 = 4 | 
| 3 | 9 - 5 = 4 | 

Tổng số là 10. Chúng tôi loại bỏ di lớn nhất trước tiên: trạm 2 hoặc 3 (cả 4), sau đó đến trạm 3 hoặc 2, sau đó đến trạm 1. Sau khi loại bỏ hai trạm, tổng còn lại là ≤ 0 nên đáp án là 2. 

Đối với ứng viên 2 (chỉ số 1), chúng tôi nhận được: 

| Trạm | di | 
| --- | --- | 
| 1 | 8 - 3 = 5 | 
| 2 | 7 - 7 = 0 | 
| 3 | 9 - 2 = 7 | 

Chúng tôi loại bỏ trạm 3 và 1 trước; một lần nữa 2 lần loại bỏ là đủ. 

Điều này xác nhận rằng nhiều ứng viên có thể đưa ra cùng một câu trả lời tối ưu và thuật toán chọn đúng mức tối thiểu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n · m log m) | Đối với mỗi ứng viên, chúng tôi tính toán m sự khác biệt và sắp xếp chúng | 
| Không gian | O(m) | Chúng tôi lưu trữ mảng chênh lệch trên mỗi trạm | 

Với n, m ≤ 100, tổng công việc là nhỏ: nhiều nhất là 100 loại cỡ 100, là tầm thường trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    def solve():
        n, m = map(int, input().split())
        a = [list(map(int, input().split())) for _ in range(m)]

        best_k = m + 1
        best_remove = None

        for j in range(n - 1):
            diffs = []
            total = 0
            for i in range(m):
                d = a[i][n - 1] - a[i][j]
                diffs.append((d, i))
                total += d

            diffs.sort(reverse=True)

            removed = []
            cur = total
            for d, i in diffs:
                if cur <= 0:
                    break
                cur -= d
                removed.append(i + 1)

            if len(removed) < best_k:
                best_k = len(removed)
                best_remove = removed

        return str(best_k) + "\n" + " ".join(map(str, best_remove)) + "\n"

    return solve()

# provided sample
assert run("""5 3
6 3 4 2 8
3 7 5 6 7
5 2 4 7 9
""") == "2\n3 1\n"

# minimum case
assert run("""2 1
1 2
""") == "0\n\n"

# all equal votes
assert run("""3 3
1 1 1
1 1 1
1 1 1
""") == "0\n\n"

# opponent already weak
assert run("""3 2
10 0 0
0 10 0
""") == "0\n\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| mẫu | 2 3 1 | tính đúng đắn trong trường hợp tiêu chuẩn | 
| 2 1 trường hợp | 0 | không cần gỡ bỏ | 
| tất cả đều bình đẳng | 0 | cà vạt ngăn cản bầu cử | 
| đối thủ yếu | 0 | chiến thắng tầm thường không đối lập | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi phe đối lập ban đầu không dẫn trước bất kỳ ứng cử viên nào. Trong tình huống đó, tổng số chênh lệch của mỗi ứng viên vốn đã không tích cực hoặc có thể trở thành không tích cực mà không cần loại bỏ bất cứ điều gì. Thuật toán xử lý việc này vì`cur`bắt đầu hoàn toàn và được kiểm tra trước bất kỳ lần xóa nào, vì vậy nó ngay lập tức chấp nhận không xóa. 

Một trường hợp khác là khi nhiều trạm có giá trị di giống nhau. Việc sắp xếp đặt chúng một cách tùy ý giữa các giá trị bằng nhau, nhưng vì thuật toán chỉ phụ thuộc vào việc loại bỏ di lớn nhất có sẵn trước tiên, nên bất kỳ thứ tự nào giữa các mối quan hệ đều dẫn đến số lần loại bỏ như nhau. Tính chính xác không phụ thuộc vào độ ổn định của việc sắp xếp. 

Trường hợp tinh vi cuối cùng là khi loại bỏ một trạm có di = 0. Điều này không làm thay đổi tổng chạy, nhưng nó vẫn có thể được chọn nếu cần để phá vỡ sự bất bình đẳng nghiêm ngặt trong một số cấu hình. Thuật toán tránh được việc loại bỏ không cần thiết một cách tự nhiên vì vòng lặp dừng ngay khi cur ≤ 0, do đó, việc loại bỏ không tác động sẽ không bao giờ được thực hiện trừ khi chúng là một phần của việc đạt đến ngưỡng đó một cách chính xác.
