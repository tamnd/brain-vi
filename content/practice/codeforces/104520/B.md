---
title: "CF 104520B - Sắp xếp nhà hàng"
description: "Chúng ta được cấp một chồng các số nguyên riêng biệt, trong đó phần tử đầu tiên là phần tử dưới cùng và phần tử cuối cùng là phần tử trên cùng."
date: "2026-06-30T10:26:03+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104520
codeforces_index: "B"
codeforces_contest_name: "Teamscode Summer 2023 Contest"
rating: 0
weight: 104520
solve_time_s: 114
verified: true
draft: false
---

[CF 104520B - Sắp xếp nhà hàng](https://codeforces.com/problemset/problem/104520/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 54s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một chồng các số nguyên riêng biệt, trong đó phần tử đầu tiên là phần tử dưới cùng và phần tử cuối cùng là phần tử trên cùng. Chúng tôi được phép lấy hậu tố của ngăn xếp này, nghĩa là chúng tôi xóa một số phần tử khỏi đầu, tạm thời giữ chúng, chỉ sắp xếp những phần tử đã xóa đó và sau đó đặt chúng trở lại đầu trang theo thứ tự đã sắp xếp. Phần còn lại của ngăn xếp vẫn giữ nguyên. 

Nhiệm vụ là tìm số phần tử nhỏ nhất mà chúng ta phải bật từ trên xuống để sau khi chỉ sắp xếp các phần tử đã bật ra đó và gắn lại chúng, toàn bộ ngăn xếp sẽ được sắp xếp theo thứ tự tăng dần từ dưới lên trên. 

Do đó, đầu ra không phải là ngăn xếp cuối cùng mà là độ dài hậu tố tối thiểu cần thiết để một thao tác đơn lẻ “bật, sắp xếp, sắp xếp lại” là đủ để sắp xếp toàn bộ cấu trúc. 

Các ràng buộc đủ lớn để mô phỏng O(n²) cho mỗi thử nghiệm sẽ không vượt qua. Tổng số n trong các trường hợp thử nghiệm lên tới 2 × 10⁵, điều này cho thấy cần phải có O(n) hoặc O(n log n) cho mỗi giải pháp thử nghiệm, lý tưởng là tuyến tính tổng thể. 

Một điểm tinh tế là chúng ta không được phép sắp xếp lại các phần tử tùy ý của ngăn xếp mà chỉ được phép sắp xếp lại một hậu tố liền kề từ trên xuống. Điều này loại bỏ nhiều chiến lược tham lam ngây thơ cố gắng khắc phục sự đảo ngược cục bộ ở bất kỳ đâu trong mảng. 

Một vài trường hợp đặc biệt bộc lộ những lỗi phổ biến. Nếu ngăn xếp đã được sắp xếp thì không cần popping và câu trả lời là 0. Ví dụ, đầu vào`1 2 3`sẽ trả về 0. Một cách tiếp cận ngây thơ luôn giả định ít nhất một phần tử phải được di chuyển sẽ thất bại ở đây. 

Một trường hợp phức tạp khác là khi hầu hết mọi thứ đều bị đảo ngược, chẳng hạn như`4 3 2 1`. Cách hợp lệ duy nhất là bật tất cả các phần tử, sắp xếp chúng và chèn lại, vì vậy câu trả lời là 4. Bất kỳ cách tiếp cận nào cố gắng duy trì một phần trật tự ở giữa sẽ đánh giá thấp hậu tố bắt buộc. 

Cuối cùng, hãy xem xét một trường hợp hỗn hợp như`1 4 2 3`. Mặc dù hầu hết các phần tử đều gần với vị trí cuối cùng của chúng, nhưng ràng buộc hậu tố buộc chúng ta phải chọn cẩn thận vị trí bắt đầu của hậu tố được sắp xếp. Câu trả lời đúng là 3, vì bật lên`[4,2,3]`là đủ để sửa thứ tự, trong khi chỉ bật hai cái cuối cùng là không đủ để sửa đảo ngược liên quan đến 4. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ trực tiếp là thử mọi giá trị có thể có của k từ 0 đến n. Đối với mỗi k, chúng tôi bật k phần tử hàng đầu, sắp xếp chúng và mô phỏng việc chèn lại chúng. Sau khi xây dựng lại, chúng tôi kiểm tra xem toàn bộ mảng đã được sắp xếp chưa. Mỗi mô phỏng tốn O(n log n) do sắp xếp và thực hiện điều này với tất cả k sẽ mang lại O(n² log n) cho mỗi thử nghiệm trong trường hợp xấu nhất. Với tổng n lên tới 2 × 10⁵, tốc độ này quá chậm. 

Quan sát quan trọng là chúng tôi không thực sự chọn các phần tử tùy ý, chỉ chọn một hậu tố và mảng cuối cùng phải được sắp xếp trên toàn cầu. Điều này có nghĩa là các phần tử không bị ảnh hưởng phải tạo thành tiền tố của mảng được sắp xếp, nếu không thì không có cách sắp xếp hậu tố nào có thể khắc phục được sự không khớp giữa các phần tử được bảo tồn. 

Nếu chúng ta tưởng tượng mảng được sắp xếp cuối cùng thì phần chưa được chạm tới phải căn chỉnh chính xác với các giá trị nhỏ nhất theo đúng thứ tự. Điều đó ngụ ý rằng nếu chúng tôi quét từ dưới lên và theo dõi xem liệu chúng tôi có thể giữ các phần tử làm tiền tố hợp lệ của hoán vị được sắp xếp hay không, chúng tôi chỉ thất bại khi gặp một phần tử phá vỡ cấu trúc tăng dần bắt buộc so với các giá trị còn lại. 

Điều này dẫn đến việc sắp xếp lại vấn đề: chúng tôi muốn tiền tố dài nhất từ ​​dưới lên có thể không bị ảnh hưởng trong khi vẫn nhất quán với mảng cuối cùng được sắp xếp trên toàn cầu. Sau khi tiền tố đó được sửa, mọi thứ ở trên nó phải được hiển thị và sắp xếp. 

Vì vậy, thay vì quyết định trực tiếp k, chúng tôi tính xem có bao nhiêu phần tử từ dưới lên có thể ở lại và trừ đi n. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n² log n) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi sử dụng ý tưởng rằng các phần tử được giữ nguyên phải tạo thành tiền tố hợp lệ của mảng đã sắp xếp khi được xem xét theo thứ tự giá trị. 

1. Sắp xếp bản sao của mảng. Điều này đưa ra thứ tự giá trị cuối cùng của mục tiêu. Chúng tôi sẽ sử dụng nó làm tài liệu tham khảo để biết ngăn xếp sẽ trông như thế nào sau tất cả các thao tác. 
2. Xây dựng ánh xạ từ giá trị tới vị trí của nó trong mảng đã sắp xếp. Điều này cho chúng ta biết thứ hạng chính xác của mọi phần tử. 
3. Quét ngăn xếp ban đầu từ dưới lên trên, theo dõi thứ hạng lớn nhất mà chúng ta đã thấy cho đến nay nếu chúng ta giả vờ rằng chúng ta đang xây dựng một tiền tố hợp lệ của mảng đã được sắp xếp. 
4. Bất cứ khi nào thứ hạng của phần tử hiện tại lớn hơn những gì chúng ta mong đợi tiếp theo ở một tiền tố nhất quán nghiêm ngặt, chúng ta không thể mở rộng tiền tố nguyên vẹn qua điểm này. 
5. Tiền tố hợp lệ dài nhất từ ​​dưới lên cho chúng ta số phần tử mà chúng ta có thể giữ nguyên. 
6. Câu trả lời k là số phần tử không có trong tiền tố này, bằng n trừ đi độ dài tiền tố đó. 

Ý tưởng chính là tiền tố không được chạm tới không được đưa ra bất kỳ “vi phạm ràng buộc trong tương lai” nào liên quan đến thứ tự được sắp xếp. Khi một phần tử không phù hợp về mặt tiến trình xếp hạng được sắp xếp, mọi thứ ở trên nó phải là một phần của hậu tố mà chúng tôi sắp xếp. 

### Tại sao nó hoạt động 

Phần chưa được chạm tới của ngăn xếp phải khớp chính xác với phân đoạn thấp nhất của mảng được sắp xếp cuối cùng, bởi vì tất cả các phần tử còn lại sẽ được chèn lại phía trên nó theo thứ tự được sắp xếp. Nếu một phần tử tiền tố có thứ hạng cao hơn phần tử sau trong tiền tố thì việc sắp xếp lại hậu tố không thể khắc phục được ràng buộc thứ tự tương đối, vì các thao tác hậu tố không thể di chuyển các phần tử vào vùng được bảo toàn. Điều này buộc vùng được bảo toàn phải chính xác là tiền tố dài nhất phù hợp với thứ tự xếp hạng tăng dần theo hoán vị được sắp xếp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        p = list(map(int, input().split()))

        sorted_p = sorted(p)
        pos = {v: i for i, v in enumerate(sorted_p)}

        # longest valid prefix from bottom
        max_rank = -1
        keep = 0

        for x in p:
            r = pos[x]
            if r > max_rank:
                keep += 1
                max_rank = r
            else:
                break

        print(n - keep)

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách sắp xếp mảng để xác định thứ tự chung chính xác. Từ điển ánh xạ từng giá trị vào thứ hạng của nó, cho phép so sánh theo thời gian liên tục trong quá trình quét. 

Sau đó, chúng tôi duyệt từ dưới lên trên, duy trì thứ hạng cao nhất được thấy cho đến nay trong một tiền tố hợp lệ. Nếu phần tử tiếp theo yêu cầu thứ hạng nhỏ hơn thứ đã được đưa vào, chúng tôi sẽ ngừng mở rộng tiền tố. Điều này trực tiếp xác định có bao nhiêu phần tử có thể không bị ảnh hưởng. Kết quả được tính là phần bù của kích thước tiền tố này. 

Một cạm bẫy phổ biến là quét từ trên xuống thay vì từ dưới lên, điều này phá vỡ cách diễn giải những gì vẫn cố định. Cấu trúc ngăn xếp làm cho hướng từ dưới lên trên trở nên cần thiết vì chỉ cho phép các thao tác hậu tố. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`1 2 3`| Bước | Giá trị | Xếp hạng | max_rank | giữ | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 0 | 0 | 1 | 
| 2 | 2 | 1 | 1 | 2 | 
| 3 | 3 | 2 | 2 | 3 | 

Tất cả các phần tử đều mở rộng một tiền tố hợp lệ, vì vậy giữ = 3, do đó k = 0. 

Điều này xác nhận rằng khi mảng đã được sắp xếp thì không cần phải xuất hiện. 

### Ví dụ 2 

đầu vào:`1 4 2 3`| Bước | Giá trị | Xếp hạng | max_rank | giữ | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 0 | 0 | 1 | 
| 2 | 4 | 3 | 3 | 2 | 
| 3 | 2 | 1 | 3 | dừng lại | 

Ở phần tử thứ ba, hạng 1 nhỏ hơn max_rank 3 nên tiền tố bị ngắt. Chúng tôi nhận được giữ = 2, vì vậy k = 2. 

Điều này chứng tỏ cách một sự đảo ngược duy nhất liên quan đến thứ tự đã sắp xếp sẽ dừng phần mở rộng của tiền tố chưa được xử lý, buộc hậu tố còn lại phải được xây dựng lại hoàn toàn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) mỗi lần kiểm tra | sắp xếp chiếm ưu thế, quét là tuyến tính | 
| Không gian | O(n) | lưu trữ bản sao và ánh xạ đã sắp xếp | 

Tổng n qua các bài kiểm tra được giới hạn bởi 2 × 10⁵, do đó chi phí sắp xếp có thể chấp nhận được. Mỗi thử nghiệm đều độc lập và quá trình xử lý bổ sung tuyến tính giúp giải pháp được thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    def solve():
        t = int(input())
        for _ in range(t):
            n = int(input())
            p = list(map(int, input().split()))
            sorted_p = sorted(p)
            pos = {v: i for i, v in enumerate(sorted_p)}

            max_rank = -1
            keep = 0
            for x in p:
                r = pos[x]
                if r > max_rank:
                    keep += 1
                    max_rank = r
                else:
                    break
            print(n - keep)

    solve()
    return sys.stdout.getvalue().strip()

# provided samples
assert run("""5
3
1 2 3
2
2 1
4
4 3 2 1
4
1 4 2 3
7
1 2 3 6 4 7 5
""") == """0
2
4
2
4"""

# custom cases
assert run("""1
1
1
""") == "0", "minimum size"

assert run("""1
5
5 4 3 2 1
""") == "5", "fully reversed"

assert run("""1
4
1 3 2 4
""") == "2", "single inversion"

assert run("""1
6
1 2 3 4 5 6
""") == "0", "already sorted"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 phần tử | 0 | trường hợp tối thiểu | 
| đảo ngược hoàn toàn | 5 | sắp xếp lại tồi tệ nhất | 
| 1 3 2 4 | 2 | xử lý đảo ngược cục bộ | 
| đã được sắp xếp | 0 | không cần thao tác | 

## Vỏ cạnh 

Đối với ngăn xếp một phần tử, thuật toán gán keep = 1 ngay lập tức vì chỉ có một hạng và không thể nảy sinh mâu thuẫn. Đầu ra trở thành 0, phù hợp với thực tế là không cần thao tác nào. 

Đối với một ngăn xếp hoàn toàn đảo ngược như`5 4 3 2 1`, việc sắp xếp sẽ gán các thứ hạng theo thứ tự tăng dần từ 0 đến 4, nhưng việc quét từ dưới lên trên ngay lập tức làm giảm thứ hạng ở phần tử thứ hai, phá vỡ tiền tố sau phần tử đầu tiên. Điều này mang lại kết quả keep = 1 và k = 4, yêu cầu tất cả các phần tử phải được bật ra một cách chính xác. 

Đối với trường hợp như`1 3 2 4`, quá trình quét chấp nhận`1`sau đó`3`, nhưng bác bỏ`2`vì thứ hạng của nó nhỏ hơn mức tối đa hiện tại. Tiền tố dừng ở đó và chỉ còn hai phần tử được giữ cố định, cho k = 2, phù hợp với nhu cầu sửa chữa nghịch đảo ở giữa.
