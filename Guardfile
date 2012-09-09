require 'guard/guard'

module ::Guard
  class Bacon < Guard
    def run_all
      system "bundle exec bacon -Itest -q -a"
      puts
      true
    end

    def run_spec(path)
      if File.exists?(path)
        success = system "bundle exec bacon -Itest -q #{path}"

        if !success
          @any_failed = true
        end

        puts
      end
    end

    def file_changed(path)
      run_spec(path)
    end

    def run_on_changes(paths)
      @any_failed = false
      paths.delete(:all)

      paths.each do |path|
        file_changed(path)
      end

      if !@any_failed
        run_all
      end
    end
  end
end

guard 'bacon' do
  # Example of mapping a lib file to one or more test files
  watch('lib/pry/indent.rb') { 'test/test_indent.rb' }

  # If no such mapping exists, just run all of them
  watch(%r{^lib/}) { :all }

  # If we modified one test file, run it
  watch(%r{^test.*/test_.+\.rb$})
end
