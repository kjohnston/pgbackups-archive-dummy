require "newrelic_rpm"
require "pg"
require "pgbackups-archive"
require "securerandom"

# spec = Gem::Specification.find_by_name "pgbackups-archive"
# load "#{spec.gem_dir}/lib/tasks/pgbackups_archive.rake"

namespace :db do

  desc "Generate a table with dummy records"
  task :generate do

    if ENV["DATABASE_URL"]
      db = URI.parse(ENV["DATABASE_URL"])
      dbname   = db.path[1..-1]
      host     = db.host
      user     = db.user
      password = db.password
    else
      dbname   = "pgbackups_archive_dummy"
      host     = "localhost"
      user     = ENV["USER"]
      password = ""
    end

    conn = PG.connect(dbname: dbname, host: host, user: user, password: password)

    puts "Creating `records` table..."
    conn.exec("DROP TABLE records")
    conn.exec("CREATE TABLE IF NOT EXISTS records (content text)")

    # Create 1000 1MB records to produce a 1GB database
    content = SecureRandom.base64(750000)
    total   = 1000

    total.times do |index|
      print "\rPopulating record #{index+1}/1000"
      conn.exec("INSERT INTO records (content) VALUES ('#{content}')")
    end

    count = conn.exec("SELECT COUNT(*) FROM records")
    puts "\nCreated #{count.first["count"]} records"

  end
end

class RunPgbackupsArchive
  include NewRelic::Agent::Instrumentation::ControllerInstrumentation

  def call
    Heroku::Client::PgbackupsArchive.perform
  end

  add_transaction_tracer :call, category: :task
end

namespace :pgbackups do

  desc "Perform a pgbackups backup then archive to S3."
  task :archive do
    RunPgbackupsArchive.new.call
  end

end

NewRelic::Agent.manual_start app_name: "pgbackups-archive-dummy",
  transaction_tracer: { transaction_threshold: 1.5 }
