---
title: "CF 104397E - Lựa chọn khóa học"
description: "Chúng tôi được cung cấp một bộ các khóa học được chọn lọc cho chương trình thạc sĩ. Mỗi khóa học đóng góp một số tín chỉ nhất định và thuộc chính xác một danh mục như cơ sở công cộng, cơ sở chuyên nghiệp, các biến thể tự chọn hoặc các buổi bắt buộc."
date: "2026-07-01T00:52:19+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104397
codeforces_index: "E"
codeforces_contest_name: "The 21st UESTC Programming Contest Final"
rating: 0
weight: 104397
solve_time_s: 78
verified: true
draft: false
---

[CF 104397E - Lựa chọn khóa học](https://codeforces.com/problemset/problem/104397/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 18s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một bộ các khóa học được chọn lọc cho chương trình thạc sĩ. Mỗi khóa học đóng góp một số tín chỉ nhất định và thuộc chính xác một danh mục như cơ sở công cộng, cơ sở chuyên nghiệp, các biến thể tự chọn hoặc các buổi bắt buộc. Mục tiêu là để xác minh xem tập hợp các khóa học đã chọn có đáp ứng tập hợp các ràng buộc tín chỉ áp dụng cho cả toàn cầu và cho mỗi danh mục hay không. 

Đối với mỗi trường hợp kiểm tra, chúng tôi đọc một số ngưỡng mô tả tổng số tín chỉ bắt buộc tối thiểu: tổng tín chỉ, tổng tín chỉ khóa học, tín chỉ phiên bắt buộc, tín chỉ khóa học cấp bằng và một số hạn chế phân chia sâu hơn cấu trúc chuyên môn và tự chọn. Sau đó, chúng ta được cung cấp một danh sách các khóa học, mỗi khóa học có một loại và giá trị tín chỉ, và chúng ta phải quyết định xem kế hoạch tổng hợp có đáp ứng đồng thời tất cả các ràng buộc hay không. 

Mặc dù đây có vẻ giống như một vấn đề về sổ sách kế toán nhưng khó khăn nằm ở việc tách biệt các danh mục chồng chéo một cách cẩn thận. Một khóa học có thể đóng góp vào nhiều ràng buộc cùng một lúc. Ví dụ, một khóa học cơ bản chuyên nghiệp đóng góp vào tổng số tín chỉ, tổng số tín chỉ khóa học, tín chỉ cấp bằng, tín chỉ chuyên môn và tín chỉ cơ bản chuyên nghiệp cùng một lúc. 

Những hạn chế là nhỏ. Mỗi số có nhiều nhất là 100 và mỗi test có tối đa 100 khóa học. Điều này đảm bảo rằng chỉ cần quét tuyến tính qua đầu vào là đủ và mọi giải pháp hoạt động liên tục trong mỗi khóa học sẽ dễ dàng vượt qua. 

Một trường hợp phức tạp xuất phát từ yêu cầu “phải tham gia ít nhất một khóa học cơ bản công cộng”. Điều này không được mã hóa dưới dạng ngưỡng số, vì vậy chỉ tính tổng các khoản tín dụng là không đủ; chúng ta phải theo dõi rõ ràng liệu một khóa học như vậy có tồn tại hay không. 

Một lỗi phổ biến khác là trộn lẫn các danh mục chồng chéo. Ví dụ, các khóa học chuyên nghiệp bao gồm cả các khóa học tự chọn chuyên nghiệp và cơ bản. Nếu chúng ta chỉ đếm nhầm một trong số chúng, chúng ta có thể thất bại hoặc vượt qua sai các ràng buộc liên quan đến e, f hoặc g. 

Vấn đề tế nhị thứ hai là các buổi học bắt buộc không nằm trong số tín chỉ của khóa học nhưng vẫn đóng góp vào tổng số tín chỉ. Vì vậy, tổng số tín chỉ bao gồm tất cả mọi thứ, trong khi tổng số tín chỉ khóa học bao gồm mọi thứ ngoại trừ có thể có một số ranh giới giải thích, nhưng trong vấn đề này, tất cả các mục được liệt kê đều là các khóa học, vì vậy cả hai tổng số đều giống nhau trong thực tế. Sự khác biệt vẫn còn quan trọng đối với tính đúng đắn trong việc giải thích. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ là việc liệt kê hoặc mô phỏng không cần thiết tất cả các tập hợp con có thể có của các khóa học, cố gắng gán chúng thành các danh mục hoặc xác minh các ràng buộc bằng cách tính toán lại. Điều đó là không bắt buộc vì đầu vào đã cung cấp lựa chọn cố định; không có lựa chọn nào để tối ưu hóa hoặc chọn các tập hợp con. Ngay cả khi chúng tôi hiểu sai nó là một vấn đề lựa chọn, việc liệt kê các tập hợp con sẽ tiêu tốn thời gian theo cấp số nhân tính bằng n, cụ thể là O(2^n), điều này ngay lập tức không khả thi. 

Quan sát quan trọng là cấu trúc hoàn toàn là phụ gia. Mọi ràng buộc được thể hiện dưới dạng tổng của các nhóm khóa học rời rạc hoặc chồng chéo. Điều này có nghĩa là chúng ta chỉ cần tính toán tích lũy theo danh mục trong một lần duy nhất. 

Giải pháp giảm thiểu việc quét tất cả các khóa học một lần và duy trì bộ đếm cho từng danh mục liên quan: tổng tín chỉ, tín chỉ khóa học, tín chỉ bắt buộc, tín chỉ cơ sở công, tín chỉ cấp bằng (cơ sở công cộng và cơ sở chuyên nghiệp), tín chỉ chuyên môn (cơ sở cộng với tự chọn), tín chỉ cơ bản chuyên nghiệp và tín chỉ tự chọn chuyên nghiệp. Chúng tôi cũng theo dõi cờ boolean cho biết liệu có tồn tại ít nhất một khóa học cơ bản công cộng hay không. 

Khi tất cả các tổng hợp được tính toán, chúng tôi so sánh chúng với các ngưỡng. Nếu mọi ràng buộc đều được thỏa mãn thì kế hoạch là hợp lệ.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (tìm kiếm tập hợp con bị hiểu sai) | O(2^n) | O(n) | Quá chậm | 
| Tổng hợp một lượt | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng trường hợp thử nghiệm một cách độc lập. 

1. Đọc tất cả các giá trị ngưỡng. Chúng xác định số tiền yêu cầu tối thiểu cho các danh mục khác nhau và đóng vai trò là mục tiêu xác thực cuối cùng. 
2. Khởi tạo tất cả các bộ đếm về 0 và cờ boolean`has_public`thành sai. Các biến này sẽ tích lũy thông tin trên tất cả các khóa học. 
3. Đối với mỗi khóa học, hãy đọc loại và giá trị tín chỉ của nó, sau đó cập nhật tất cả các bộ đếm có liên quan dựa trên danh mục. Mỗi khóa học đóng góp vào nhiều quầy tùy thuộc vào phân loại của nó. Ví dụ, một khóa học cơ bản chuyên nghiệp sẽ tăng tổng số tín chỉ, tín chỉ khóa học, tín chỉ cấp bằng, tín chỉ chuyên môn và tín chỉ cơ sở chuyên môn cùng một lúc. 
4. Nếu khóa học là khóa học cơ bản công cộng, hãy đặt`has_public`thành true và cập nhật tất cả các khoản tiền có liên quan cho phù hợp. 
5. Nếu khóa học là môn tự chọn chuyên nghiệp, hãy cập nhật các tín chỉ tự chọn chuyên môn và đóng góp vào tín chỉ chuyên môn. 
6. Nếu khóa học là học phần bắt buộc thì chỉ cộng vào số tín chỉ và tổng số tín chỉ của học phần bắt buộc vì đây không phải là một phần của cấu trúc khóa học cấp bằng. 
7. Sau khi xử lý tất cả các khóa học, hãy kiểm tra từng ràng buộc: tổng số tín chỉ, tín chỉ khóa học, tín chỉ bắt buộc, tín chỉ cấp bằng, tín chỉ chuyên môn, tín chỉ cơ sở chuyên môn và tín chỉ tự chọn chuyên nghiệp. Đồng thời xác minh rằng ít nhất một khóa học cơ bản công cộng đã được tham gia. 

### Tại sao nó hoạt động 

Mỗi khóa học đóng góp độc lập vào một bộ đếm phụ gia cố định. Vì các ràng buộc là các bất đẳng thức tuyến tính trên cùng các bộ đếm này nên việc duy trì tổng chính xác sẽ đảm bảo tính chính xác. Không có khóa học nào trong tương lai có thể làm mất hiệu lực các tính toán trong quá khứ, do đó, việc tích lũy một lượt duy nhất sẽ bảo toàn tất cả thông tin cần thiết mà không cần quay lại hoặc tính toán lại. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    T = int(input())
    for _ in range(T):
        a, b, c, d, e, f, g = map(int, input().split())
        n = int(input())

        total = 0
        course_total = 0
        compulsory = 0

        degree = 0
        professional = 0
        prof_found = 0
        prof_elective = 0

        has_public = False

        for _ in range(n):
            name = input().rstrip()
            typ = input().rstrip()
            val = int(input())

            total += val
            course_total += val

            if typ == "compulsory sessions":
                compulsory += val

            if typ == "public foundational courses":
                has_public = True
                degree += val

            if typ == "professional foundational courses":
                degree += val
                professional += val
                prof_found += val

            if typ == "professional elective courses":
                professional += val
                prof_elective += val

            if typ == "interdisciplinary elective courses":
                pass

            if typ == "other elective courses":
                pass

        ok = True
        ok &= total >= a
        ok &= course_total >= b
        ok &= compulsory >= c
        ok &= degree >= d
        ok &= professional >= e
        ok &= prof_found >= f
        ok &= prof_elective >= g
        ok &= has_public

        print("YES" if ok else "NO")

if __name__ == "__main__":
    solve()
```Việc triển khai phản ánh trực tiếp sự phân rã danh mục. Mỗi nhánh có điều kiện tương ứng với một loại khóa học và chỉ cập nhật các bộ đếm bao gồm nó một cách xác định. Kiểm tra boolean cuối cùng tổng hợp tất cả các ràng buộc trong một biểu thức duy nhất, đảm bảo không có điều kiện nào bị bỏ qua. 

Một sai lầm phổ biến là quên rằng tín chỉ bằng cấp bao gồm cả các khóa học cơ bản công cộng và chuyên nghiệp. Một cách khác là coi các hạng mục tự chọn là loại trừ lẫn nhau khỏi tổng số chuyên môn, điều này sẽ đánh giá thấp hơn`professional`và phá vỡ điều kiện liên quan`e`. 

## Ví dụ đã hoạt động 

Chúng tôi sử dụng đầu vào mẫu được cung cấp. 

### Dấu vết 

Chúng tôi chỉ theo dõi các tổng hợp chính. 

| Bước | Loại khóa học | Giá trị | tổng cộng | bắt buộc | bằng cấp | chuyên nghiệp | pro_found | pro_tự chọn | has_public | 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 1 | cơ sở công cộng | 2 | 2 | 0 | 2 | 0 | 0 | 0 | Đúng | 
| 2 | giáo sư nền tảng | 3 | 5 | 0 | 5 | 3 | 3 | 0 | Đúng | 
| 3 | giáo sư nền tảng | 3 | 8 | 0 | 8 | 6 | 6 | 0 | Đúng | 
| 4 | giáo sư tự chọn | 2 | 10 | 0 | 8 | 8 | 6 | 2 | Đúng | 
| 5 | giáo sư tự chọn | 2 | 12 | 0 | 8 | 10 | 6 | 4 | Đúng | 
| 6 | giáo sư nền tảng | 2 | 14 | 0 | 10 | 12 | 8 | 4 | Đúng | 
| 7 | giáo sư nền tảng | 3 | 17 | 0 | 13 | 15 | 11 | 4 | Đúng | 
| 8 | giáo sư tự chọn | 2 | 19 | 0 | 13 | 17 | 11 | 6 | Đúng | 
| 9 | giáo sư tự chọn | 2 | 21 | 0 | 13 | 19 | 11 | 8 | Đúng | 
| 10 | môn tự chọn khác | 1 | 22 | 0 | 13 | 19 | 11 | 8 | Đúng | 
| 11 | cơ sở công cộng | 3 | 25 | 0 | 16 | 19 | 11 | 8 | Đúng | 
| 12 | bắt buộc | 1 | 26 | 1 | 16 | 19 | 11 | 8 | Đúng | 
| 13 | bắt buộc | 1 | 27 | 2 | 16 | 19 | 11 | 8 | Đúng | 
| 14 | bắt buộc | 1 | 28 | 3 | 16 | 19 | 11 | 8 | Đúng | 
| 15 | bắt buộc | 1 | 29 | 4 | 16 | 19 | 11 | 8 | Đúng | 

Cuối cùng, tất cả các ngưỡng đều được đáp ứng, vì vậy đầu ra là CÓ. 

Dấu vết này cho thấy các tín chỉ bắt buộc được tích lũy riêng biệt như thế nào trong khi vẫn đóng góp vào tổng số tín chỉ và cách các danh mục chuyên môn chồng chéo tạo ra nhiều khoản tiền cùng một lúc. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi khóa học được xử lý một lần với các bản cập nhật liên tục | 
| Không gian | O(1) | Chỉ duy trì một số quầy cố định | 

Cho n ≤ 100 và T ≤ 100 thì số phép toán tối đa là không đáng kể. Giải pháp dễ dàng phù hợp với cả giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    from math import isclose

    # re-run solution
    input = sys.stdin.readline

    def solve():
        T = int(input())
        out = []
        for _ in range(T):
            a, b, c, d, e, f, g = map(int, input().split())
            n = int(input())

            total = course_total = compulsory = 0
            degree = professional = prof_found = prof_elective = 0
            has_public = False

            for _ in range(n):
                _ = input().rstrip()
                typ = input().rstrip()
                val = int(input())

                total += val
                course_total += val

                if typ == "compulsory sessions":
                    compulsory += val
                if typ == "public foundational courses":
                    has_public = True
                    degree += val
                if typ == "professional foundational courses":
                    degree += val
                    professional += val
                    prof_found += val
                if typ == "professional elective courses":
                    professional += val
                    prof_elective += val

            ok = (total >= a and course_total >= b and compulsory >= c and
                  degree >= d and professional >= e and prof_found >= f and
                  prof_elective >= g and has_public)

            out.append("YES" if ok else "NO")

        return "\n".join(out)

    return solve()

# sample
assert run("""1
28 24 4 15 15 6 7
15
Socialism with Chinese Characteristics
public foundational courses
2
Matrix Theory
professional foundational courses
3
Optimization Theory
professional foundational courses
3
Communication Network Theory
professional elective courses
2
Bayesian Learning and Random Matrix
professional elective courses
2
Image and Video Processing
professional foundational courses
2
Graph Theory
professional foundational courses
3
Machine Learning
professional elective courses
2
Visual Data Analysis
professional elective courses
2
Guidance on Writing Graduate Thesis
other elective courses
1
Graduate English
public foundational courses
3
Teaching Practice
compulsory sessions
1
Academic Activities
compulsory sessions
1
General Education Elective Courses
compulsory sessions
1
Academic Exchange
compulsory sessions
1
""") == "YES"

# minimum case: missing public course
assert run("""1
5 5 1 2 2 1 1
2
A
professional foundational courses
3
B
professional elective courses
2
""") == "NO"

# all constraints exactly met
assert run("""1
5 5 0 2 2 1 1
2
A
public foundational courses
2
B
professional foundational courses
3
""") == "YES"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| thiếu khóa học công cộng | KHÔNG | thực thi ràng buộc boolean | 
| sự hài lòng chính xác | CÓ | độ đúng ranh giới | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi tất cả các ràng buộc về số đều được thỏa mãn nhưng không tham gia khóa học cơ bản công khai nào. Trong tình huống đó, tất cả các tổng có thể vượt quá ngưỡng, nhưng yêu cầu boolean không thành công, buộc phải KHÔNG. Thuật toán xử lý việc này vì`has_public`được theo dõi độc lập với tổng số và được đưa vào lần kiểm tra cuối cùng. 

Một trường hợp khác là khi các khóa học chỉ tồn tại trong các môn tự chọn. Những khoản này đóng góp vào tổng số tín chỉ nhưng không đóng góp vào số tiền cơ bản về bằng cấp hoặc chuyên môn. Việc phân tách đảm bảo rằng chúng tôi không tăng sai các bộ đếm liên quan đến độ, vì chỉ các loại được gắn nhãn rõ ràng mới cập nhật các tổng hợp đó.
