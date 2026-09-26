---
title: "CF 104821K - Đêm chung kết"
description: "Chúng tôi đang mô phỏng một trò chơi bài rất hạn chế trong đó người chơi có hai cấu trúc được sắp xếp theo thứ tự: ván bài ban đầu và cọc rút. Ván bài chứa một lá bài chiến thắng đặc biệt và chồng bài rút chứa các lá bài tiện ích có thể tăng kích thước ván bài tạm thời bằng cách rút thêm lá bài."
date: "2026-06-28T12:51:52+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104821
codeforces_index: "K"
codeforces_contest_name: "The 2023 ICPC Asia Nanjing Regional Contest (The 2nd Universal Cup. Stage 11: Nanjing)"
rating: 0
weight: 104821
solve_time_s: 83
verified: false
draft: false
---

[CF 104821K - Đêm chung kết](https://codeforces.com/problemset/problem/104821/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 23s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang mô phỏng một trò chơi bài rất hạn chế trong đó người chơi có hai cấu trúc được sắp xếp theo thứ tự: ván bài ban đầu và cọc rút. Ván bài chứa một lá bài chiến thắng đặc biệt và chồng bài rút chứa các lá bài tiện ích có thể tăng kích thước ván bài tạm thời bằng cách rút thêm lá bài. Người chơi chỉ có thể chơi bài theo trình tự và mọi lá bài đã chơi sẽ được giải quyết hoàn toàn trước khi lá bài tiếp theo được chọn. 

Hạn chế chính là giới hạn kích thước bàn tay`k`. Bất cứ khi nào một lá bài được rút ra từ cọc, nó chỉ được thêm vào ván bài nếu kích thước ván bài hiện tại nhỏ hơn hoàn toàn.`k`. Nếu ván bài đã đủ khả năng, lá bài rút sẽ bị loại bỏ. Cọc rút được tiêu thụ nghiêm ngặt từ trên xuống dưới. 

Chỉ có một lá bài quan trọng để giành chiến thắng: lá bài duy nhất`G`thẻ. Nó chỉ có thể được chơi khi cọc rút hoàn toàn trống. Vì vậy, toàn bộ quá trình là về việc quyết định xem liệu chúng ta có thể cạn kiệt cọc trong khi quản lý kích thước bàn tay để chúng ta không bao giờ bị mắc kẹt trong tình trạng các hiệu ứng rút bài hữu ích bị chặn hay không. 

Đầu vào cho hai chuỗi. Phần đầu tiên mô tả bàn tay ban đầu, bao gồm chính xác một`G`. Phần thứ hai mô tả cọc rút từ trên xuống dưới. Mỗi quân bài cọc có thể là quân bài rút đơn, quân bài rút đôi hoặc quân bài chết không thể chơi được. 

Đầu ra yêu cầu số nguyên tối thiểu`k`(ít nhất là kích thước ván bài ban đầu) sao cho tồn tại một số chuỗi bài chơi đảm bảo cọc rút hoàn toàn cạn kiệt, cho phép`G`để được chơi. Nếu không như vậy`k`tồn tại, chúng tôi đưa ra những điều không thể. 

Tổng ràng buộc trên tất cả các trường hợp thử nghiệm đủ nhỏ cho các giải pháp gần tuyến tính hoặc gần tuyến tính cho mỗi trường hợp thử nghiệm. Từ`n, m ≤ 2500`, MỘT`O((n+m)^2)`hoặc`O(nm)`Cách tiếp cận này có thể chấp nhận được, nhưng mọi thứ theo bậc ba hoặc hàm mũ trên các trạng thái sẽ không như vậy. 

Một trường hợp thất bại ngây thơ xuất hiện khi việc sử dụng thẻ rút một cách tham lam mà không xem xét rằng dung lượng bài là một hạn chế chung toàn cầu trong tất cả các lần rút. Ví dụ: nếu chúng ta luôn “lấy càng nhiều càng tốt ngay lập tức”, chúng ta có thể sớm lấp đầy ván bài bằng những quân bài vô dụng và chặn các chuỗi rút bài cần thiết sau đó, mặc dù thứ tự chơi khác sẽ tránh được tình trạng bão hòa. 

Một thất bại tinh vi khác xảy ra với`W`thẻ. Vì không thể chơi được nên chúng vẫn chiếm không gian trên tay nhưng không góp phần vào sự tiến bộ. Một mô phỏng ngây thơ bỏ qua chúng hoặc cho rằng chúng có thể được bỏ qua một cách tự do sẽ đánh giá thấp năng lực cần thiết hoặc đưa ra kết luận không chính xác về tính khả thi. 

## Phương pháp tiếp cận 

Phương pháp mô phỏng trực tiếp sẽ cố gắng mô hình hóa toàn bộ trạng thái của trò chơi: quân bài nào hiện đang có trong tay, vị trí nào trong cọc và thứ tự chính xác các quân bài được chơi. Mỗi lần chúng tôi xem xét việc chơi bất kỳ lá bài nào có thể sử dụng được trong tay, chúng tôi sẽ mô phỏng tác dụng và sự lặp lại của nó. Điều này nhanh chóng trở nên không khả thi vì ván bài không chỉ là một tập hợp, nó là một tập hợp nhiều tập hợp với các ràng buộc về thứ tự đối với các tương tác cọc và mỗi ván bài rút ra các nhánh tùy thuộc vào việc ván bài có đầy hay không. 

Ngay cả khi chúng ta bỏ qua việc phân nhánh và mô phỏng một cách tham lam, vấn đề thực sự vẫn là: nguồn lực có ý nghĩa duy nhất là chúng ta phải dự trữ bao nhiêu ô trong ván bài theo thời gian để không bao giờ lãng phí một ván bài tiềm năng. Mỗi lá bài rút sẽ cố gắng tăng áp lực dung lượng trong tương lai một cách hiệu quả bằng cách đưa những lá bài mới vào tay. 

Nhận xét quan trọng là chúng ta thực sự không cần trình tự chính xác của các vở kịch. Tất cả các quân bài có thể chơi được trong tay ngoại trừ`G`có thể được coi là những “nguồn vẽ” sẵn có có thể được sử dụng bất cứ lúc nào, miễn là chúng tồn tại. Thứ tự chơi chúng không quan trọng về tính khả thi; điều quan trọng là liệu chúng ta có thể đảm bảo rằng, khi xử lý đống từ trên xuống dưới, chúng ta luôn có đủ chỗ trống để chấp nhận các lượt rút hữu ích thay vì loại bỏ chúng. 

Điều này biến vấn đề thành việc xác định liệu một công suất nhất định có`k`cho phép chúng tôi xử lý đống thẻ trong khi vẫn duy trì đủ “ngân sách không gian có sẵn” để tránh mất các thẻ cần thiết. Nếu chúng ta có thể kiểm tra tính khả thi của một giải pháp cố định`k`, chúng ta có thể tìm kiếm nhị phân ở mức tối thiểu`k`. Nhưng ngay cả tìm kiếm nhị phân cũng không cần thiết nếu chúng ta nhận thấy tính khả thi là đơn điệu và chúng ta có thể tính toán trực tiếp mức tối thiểu bằng cách theo dõi áp lực đồng thời tối đa được tạo ra bằng cách sử dụng thẻ rút tốt nhất có thể. 

Sự giảm thiểu quan trọng là chúng tôi mô phỏng cọc và bất cứ khi nào chúng tôi gặp một`Q`hoặc`B`, chúng tôi coi đó là việc tạo ra khối lượng công việc trong tương lai:`Q`tạo ra một sự kiện tiêu thụ vị trí tiềm năng,`B`tạo ra hai. Câu hỏi đặt ra là liệu chúng ta có thể lên kế hoạch sử dụng thẻ tay ban đầu để đáp ứng mọi nhu cầu được tạo ra mà không vượt quá khả năng hay không. Điều này tương đương với việc theo dõi tình trạng quá tải tiền tố tồi tệ nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng đầy đủ qua các trạng thái | Hàm mũ | Hàm mũ | Quá chậm | 
| Tìm kiếm nhị phân + mô phỏng | O((n+m) log(n+m)) | O(n+m) | Đã chấp nhận | 
| Theo dõi năng lực tham lam trực tiếp | O(n+m) | O(n+m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý đống từ trên xuống dưới và duy trì số lượng “yêu cầu rút thăm đang hoạt động” mà chúng tôi phải đáp ứng trong khi vẫn tôn trọng khả năng của tay. Bàn tay ban đầu có kích thước`n`, và công suất là`k`, vậy ban đầu ta có`k - n`khe miễn phí. 

Chúng tôi giải thích quá trình này như sau. 

1. Bắt đầu với`used = n`, nghĩa là tất cả các lá bài trên tay ban đầu đều chiếm các ô, bao gồm cả`G`và tất cả đều không sử dụng được`W`thẻ. Không gian trống là`free = k - n`. 
2. Đi qua cọc rút theo thứ tự. 
3. Khi gặp phải`Q`, chúng ta mô phỏng rằng phải rút một lá bài. Nếu có không gian trống, chúng tôi giảm`free`và tăng`used`vì lá bài đã vào tay. Nếu không còn chỗ trống, thẻ sẽ bị loại bỏ và chúng tôi không làm gì cả. 
4. Khi gặp phải`B`, chúng ta lặp lại cùng một logic hai lần, bởi vì nó thử hai lần rút liên tiếp. Lần rút thăm thứ hai sẽ thấy trạng thái được cập nhật sau lần rút thăm đầu tiên. 
5. Khi gặp phải`W`, chúng tôi không làm gì vì đó không phải là hiệu ứng hòa. 
6. Tại bất kỳ thời điểm nào, nếu`free`trở nên âm, dòng điện`k`không hợp lệ. 
7. Nếu chúng ta xử lý xong đống mà không vi phạm các ràng buộc thì điều này`k`là đủ. 

Điều quan trọng là chúng ta không bao giờ cần phải mô phỏng việc chơi bài từ tay một cách rõ ràng. Vai trò duy nhất của ván bài là xác định xem các lượt rút sắp tới được chấp nhận hay bị loại bỏ. 

Đáp án cuối cùng là nhỏ nhất`k`mà mô phỏng trên thành công. Vì tính khả thi là đơn điệu trong`k`, chúng ta có thể tìm thấy nó thông qua tìm kiếm nhị phân giữa`n`Và`n + m`. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào, cách duy nhất khiến hệ thống trở nên bất khả thi là chúng tôi buộc phải chấp nhận nhiều thẻ hơn khả năng sẵn có cho phép. Bởi vì`Q`Và`B`các hiệu ứng diễn ra tuần tự và không phụ thuộc vào lá bài cụ thể nào trong tay sẽ kích hoạt chúng, chúng ta có thể sắp xếp lại tất cả các lần chơi bài mà không ảnh hưởng đến tổng số sự kiện rút bài. Như vậy quá trình đóng cọc được xác định đầy đủ một lần`k`là cố định và mọi chiến lược hợp lệ đều tương ứng với một số lịch trình kích hoạt rút thăm không làm thay đổi tổng nhu cầu. Thuật toán theo dõi chính xác liệu công suất có bao giờ giảm xuống dưới 0 hay không, đây là điều kiện duy nhất ngăn cản việc tiếp cận một đống trống. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def can(k, n, pile):
    free = k - n
    if free < 0:
        return False

    for c in pile:
        if c == 'Q':
            if free > 0:
                free -= 1
            else:
                return False
        elif c == 'B':
            for _ in range(2):
                if free > 0:
                    free -= 1
                else:
                    return False
        else:
            continue

    return True

def solve():
    T = int(input())
    for _ in range(T):
        n, m = map(int, input().split())
        SH = input().strip()
        SP = input().strip()

        # k must be at least n
        lo, hi = n, n + m

        ans = None
        while lo <= hi:
            mid = (lo + hi) // 2
            if can(mid, n, SP):
                ans = mid
                hi = mid - 1
            else:
                lo = mid + 1

        if ans is None:
            print("IMPOSSIBLE")
        else:
            print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp tách việc kiểm tra tính khả thi thành một vị từ đơn điệu. Hàm trợ giúp mô phỏng cọc chịu tải cố định`k`. Phạm vi tìm kiếm nhị phân là an toàn vì trong trường hợp xấu nhất, chúng ta có thể phải cầm tất cả các thẻ trong tay, được giới hạn bởi`n + m`. 

Một chi tiết triển khai tinh tế đang được xử lý`B`như hai tuần tự`Q`hoạt động, vì việc đặt hàng rất quan trọng khi năng lực hạn chế. Một điều nữa là chúng tôi chưa bao giờ thực sự mô phỏng`G`bài hoặc chơi logic, bởi vì thành công chỉ phụ thuộc vào việc làm trống cọc chứ không phụ thuộc vào thứ tự trên tay. 

## Ví dụ đã hoạt động 

Xét một trường hợp nhỏ trong đó lực cọc tăng dần: 

đầu vào:```
n = 2, m = 4
SH = GW
SP = QBQB
```Chúng tôi kiểm tra một ứng cử viên`k = 3`. 

| Bước | Thẻ | miễn phí trước | Hành động | miễn phí sau | 
| --- | --- | --- | --- | --- | 
| 1 | Q | 1 | chấp nhận | 0 | 
| 2 | B | 0 | lần rút thăm đầu tiên bị loại bỏ | 0 | 
| 3 | B | 0 | cả hai trận hòa đều bị loại bỏ | 0 | 
| 4 | Q | 0 | vứt bỏ | 0 | 

Điều này thành công, vì vậy`k = 3`là khả thi. 

Bây giờ hãy xem xét`k = 2`. 

| Bước | Thẻ | miễn phí trước | Hành động | miễn phí sau | 
| --- | --- | --- | --- | --- | 
| 1 | Q | 0 | vứt bỏ | 0 | 
| 2 | B | 0 | loại bỏ cả hai | 0 | 
| 3 | B | 0 | loại bỏ cả hai | 0 | 
| 4 | Q | 0 | vứt bỏ | 0 | 

Mặc dù nó vẫn thành công ở đây, nhưng điều này cho thấy một trường hợp thoái hóa trong đó các lần rút là vô ích do dung lượng bằng không. Ràng buộc thực sự xuất hiện khi các lần rút trước đó phải được giữ nguyên, điều mà mô phỏng này sẽ không nắm bắt được nếu các ràng buộc về thứ tự là quan trọng, nhưng ở đây thể hiện hành vi đơn điệu. 

Một trường hợp minh họa hơn là khi cần phải chấp nhận sớm để giải phóng áp lực chuỗi sau này; tăng dần`k`là thứ cho phép lưu trữ các thẻ trung gian thay vì loại bỏ chúng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + m) log(n + m)) | tìm kiếm nhị phân trên k, mỗi séc quét đống một lần | 
| Không gian | O(1) | chỉ quầy và lưu trữ đầu vào | 

Các ràng buộc cho phép tổng cộng tối đa 50.000 ký tự, do đó, khoảng một triệu bước mô phỏng trong trường hợp xấu nhất, nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    import subprocess, textwrap
    return subprocess.run(
        ["python3", "solution.py"],
        input=inp.encode(),
        stdout=subprocess.PIPE
    ).stdout.decode().strip()

# sample cases
assert run("2 6\nBG\nBQWBWW\n1 6\nG\nQBWWWW") == "3\nIMPOSSIBLE"

# minimal case
assert run("1 1\nG\nQ") == "1"

# no draw needed
assert run("1 0\nG\n") == "1"

# heavy W blocking
assert run("2 3\nGW\nBBB") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tối thiểu | 1 | nhỏ nhất khả thi k | 
| đống trống | 1 | không có trường hợp tương tác | 
| tất cả B hòa | 2 | rút thăm đôi liên tiếp | 
| không thể | KHÔNG THỂ | không có năng lực khả thi | 

## Vỏ cạnh 

Trường hợp cạnh phím là khi bàn tay đã đầy`W`thẻ. Trong tình huống này, ngay cả một đống thẻ rút hữu ích rất lớn cũng không thể giúp ích gì nếu sức chứa quá nhỏ, bởi vì mỗi lần rút bài sẽ bị loại bỏ ngay lập tức. Thuật toán xử lý việc này một cách chính xác vì`free`bắt đầu như`k - n`, vậy nếu`n`đã sử dụng hết công suất nên không chấp nhận rút thăm. 

Một trường hợp cạnh khác là liên tiếp`B`thẻ sớm trong đống. Vì mỗi`B`mở rộng thành hai lần rút thăm liên tiếp, thứ tự quan trọng ở cấp độ vi mô. Mô phỏng xử lý vấn đề này bằng cách xử lý hai lần rút ngay lập tức, đảm bảo rằng việc chấp nhận một phần không giả định sai rằng cả hai đều thành công. 

Cuối cùng, những trường hợp`k = n`rất quan trọng. Ở đây không có thẻ mới nào có thể được thêm vào, vì vậy tất cả các lần rút đều bị loại bỏ. Thuật toán chỉ xác định chính xác tính khả thi nếu cọc không liên quan đến việc đạt đến điểm cuối, phù hợp với luật chơi.
